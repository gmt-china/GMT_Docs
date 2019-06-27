.. index:: ! solar

solar
=====

:官方文档: :doc:`gmt:solar`
:简介: 计算或/和绘制晨昏线以及民用、航海用以及天文用曙暮光区域

必选选项
--------

``-I`` 和 ``-T`` 中必须要有一个。

可选选项
--------

``-C``
    在一行内格式化打印（以Tab键分隔） ``-I`` 选项输出的信息。输出内容包括：

    #. 太阳的经度、纬度、方位角、高度角，单位为度
    #. 日出、日落、正午的时间，单位为天
    #. 日长，单位为分钟
    #. 考虑折射效应矫正后的太阳高度较正以及均时差，单位为分钟

    .. note::

       若没有通过 ``-Ilon/lat`` 提供经纬度，则太阳高度角之后的数据均以 (0,0)
       作为参考点

    示例::

        $ gmt pssolar -I120/40+d2016-11-01T01:00:00+z8 -C
        160.885755836	-14.5068940782	38.6719503593	-59.513608404	0.270214374769	0.706928713211	0.48857154399	628.868647356	-59.5102114599	16.4569766548

``-G<fill>|c``
    根据晨昏线对黑夜区域填充颜色或图案，见 :doc:`/basis/fill` 一节。
    也可以使用 ``-Gc`` 剪裁黑夜区域，并通过使用 ``gmt psclip -C`` 停止区域剪裁，
    见 :doc:`gmt:clip` 。

``-I[<lon>/<lat>][+d<date>][+z<TZ>]``
    输出太阳的当前位置、方位角和高度角。加上 ``<lon>/<lat>`` 则输出日出、日落、
    正午时间以及一天时间长度。用 ``+d<data>`` 指定ISO格式的日期时间
    （ 比如 ``+d2000-04-25T10:00:00`` ）来计算特定时刻的太阳参数。如果有需要，
    也可以通过 ``+z<TZ>`` 加上时区。

    ::

        $ gmt pssolar -I120/40+d2016-11-01T01:00:00+z8
            Sun current position:    long = 160.885756    lat = -14.506894
                                  Azimuth = 38.6720    Elevation = -59.5136
        Sunrise  = 06:29
        Sunset   = 16:58
        Noon     = 11:44
        Duration = 10:29

``-M``
    将晨昏线数据以多段ASCII表格式写到标准输出（或二进制格式，
    见 :doc:`/option/binary` ）。使用该选项，则只输出数据不绘图。

``-N``
    反转晨昏线“内”和“外”概念颠倒。仅可与 ``-Gc`` 一起使用以剪裁出白日区，不可
    与 ``-B`` 一同使用。

``-T<dcna>[+d<date>][+z<TZ>]``
    绘制一个或多个不同定义的晨昏线。若需要导出晨昏线数据，见 ``-M`` 选项。

    通过添加 ``dcna`` 来绘制一个或多个不同定义的晨昏线。其中，

    - ``d`` 指晨昏线
    - ``c`` 指民用曙暮光
    - ``n`` 指航海曙暮光
    - ``a`` 指天文曙暮光

    ``+d<date>`` 为ISO格式的日期时间（例如 ``+d2000-04-25T12:15:00`` ），
    以得到该时刻晨昏交替的位置。如果有需要，也可以通过 ``+z<TZ>`` 加上时区。

    不同曙暮光区的定义如下图所示：

    .. figure:: https://upload.wikimedia.org/wikipedia/commons/thumb/d/d2/Twilight_subcategories.svg/640px-Twilight_subcategories.svg.png
       :align: center
       :width: 80%

       曙暮光区的多种定义（图片来自于 https://en.wikipedia.org/wiki/Twilight）

    - 民用曙暮光分为晨间曙光区和晚间暮光区：

      - 晨间曙光区是指太阳的几何中心位于地平线以下6˚至地平线以下0˚50'（或日出，
        即太阳上边缘接触地平线）这段时间
      - 晚间曙光区是指太阳的几何中心位于地平线以下 0˚50'（或日落，即太阳下边
        缘接触地平线）至地平线以下6˚ 这段时间

    - 航海曙暮光指太阳中心位于地平线以下 0˚50' 至 12˚ 这段时间
    - 天文曙暮光指太阳中心位于地平线以下 0˚50' 至 18˚ 这段时间

    下面的命令绘制了晨昏线以及三条曙暮光线::

        $ gmt coast -Rd -W0.1p -JQ0/14c -Ba -BWSen -Dl -A1000 -P -K > terminator.ps
        $ gmt pssolar -R -J -W1p -Tdcna -O >> terminator.ps

``-W[<pen>]``
    设置晨昏线的画笔属性，见 :doc:`/basis/pen` 。

示例
----

示例见 http://gmt-china.org/example/ex009/

BUGS
----

#. ``-T+d<date>`` 在取某些值时会段错误退出（v5.3.1）