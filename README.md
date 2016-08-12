# pos-printer-api
pos-printer-api（票据打印机驱动接口）中规范了常用的票据格式和标准的连接接口，以达到无限匹配新打印机硬件的目的。票据打印机驱动接口，适合通过卷装纸输出的热敏和针式打印机。

API的核心由打印元素、控制转义、连接接口和设备设置这4部分构成。

>票据打印机的生产厂商应该实现这套标准定义，降低打印机接入软件体系的技术成本，提升产品竞争力；开发爱好者或使用打印机的开发者们共同实现并开放驱动源码，互相帮助。

# 打印元素
分成如下几个大类：

## 文本类型
文本是票据打印机需要支持的最基本输出元素，分别如下：

### Section(文本段落)
普通的段落型文本，输出后自动换到新行，类型于html中的div标签。Section由以下几个元素构成：

#### Text(文本内容)
段落中的文字都由这些文本构成，每个Text中又包含Font设置，用于控制字体。

##### Font(字体控制)
在所有需要字体控制的元素中都将使用到这个元素。其中包含如下属性：

* fontSize 字体大小：Big、Normal和Small，分别对应：大、中和小三种尺寸
* bold 是否粗体：true|false
* underline 是否下划线：true|false

#### TextAlign(排列控制)

* Left 居左
* Center 居中
* Right 居右

下面是各种排列示例

    |align left                           |
    |            align center             |
    |                          align right|

### Table(表格)
表格包含如下几个属性
* headers 表头名称
* columnWidths 列宽：每个单元格的宽度（字符数），中文算两个，英文算一个，但所有宽度之和不能大于列可输出的字数
* alignRights 列居右：设备每列的文字排列方式是否居右
>LINE_ROW 横线行，在表格中追加一条横线

示例：

    |NAME        COUNT    PRICE     AMOUNT|
    ---------------------------------------
    |apple           2     6.00      12.00|
    |cherry          4    30.00     120.00|
    |banana          5     8.00      40.00|
    ---------------------------------------
    |SUM                            172.00|

### KeyValue(键值)

打印key=value型的文档，有紧凑和分离两种排列方式。

紧凑型排列方式：

    |name: yaoming                        |
    |address: BeiJing chaoyang  anli road |
    |         118 build                   |
    
分离型排列方式：

    |name:                         yaoming|
    |address:  BeiJing chaoyang  anli road|
    |                            118 build| 
  
## 图片类型

很多POS打印机支持图片格式的输出，但并不支持横线、条形码或二维码输出，所以我们设计了以下元素，由驱动实现，以减小使用者的开发难度。

### Image(图片)
普通的bitmap格式编码的图片。

* margin 上下文间距(像素)
* width 图片宽度(素数)
* height 图片高度(像素)
* pixels 图片的点阵数据

居中输出，输出完成后自动换新行。

    |                                  |
    |       +++++++++++++++++++        |
    |       +                 +        |
    |       +  Image content  +        |
    |       +                 +        |
    |       +++++++++++++++++++        |
    |                                  |

### Line(横线)
一条直线的图片。

    |                                  |
    ------------------------------------
    |                                  |

### Barcode(条形码)

* content 条码内容
* margin 条码与上下文之间的间距(像素)
* height 条码的高度(像素)

居中输出，输出完成后自动换新行。

    |                                  |
    |       || ||||| | || ||| |        |
    |                                  |

### QRcode(二维码)

* margin 上下文间距(像素)
* content 内容
* width 宽度(像素)

居中输出，输出完成后自动换新行。

    |                                  |
    |       +++++++++++++++++++        |
    |       +                 +        |
    |       +  QRcode content +        |
    |       +                 +        |
    |       +++++++++++++++++++        |
    |                                  |

## 控制类型

### BlankRow(空行)
输出一个空行，相当于向前走纸一行。

### CutPage(切纸)
部分打印机有切纸功能，如果没有建议走纸到切纸位置并呜音。
