# pos-printer-api
pos-printer-api（票据打印机驱动接口）中规范了常用的票据格式和标准的连接接口，以达到无限匹配新打印机硬件的目的。

票据打印机驱动接口，适合通过卷装纸输出的热敏和针式打印机。

API的核心由打印元素、控制转义、连接接口和设备设置这4部分构成。

我的愿景：票据打印机的生产厂商应该实现这套标准定义，降低打印机接入软件体系的技术成本，提升产品竞争力；开发爱好者或使用打印机的开发者们共同实现并开放驱动源码，为人为已。

# 打印元素
分成如下几个大类：

## 文本类型
文本是票据打印机需要支持的最基本输出元素，分别如下：

### Section(文本段落)
普通的段落型文本，在输出后应自动换到新行，类型于html中的div标签。Section由以下几个元素构成：

#### Text(文本内容)
段落中的文字都由这些文本构成，每个Text中又包含Font设置，用于控制字体。

##### Font(字体控制)
在所有需要字体控制的元素中都将使用到这个元素。其中包含如下属性：

>fontSize 字体大小：Big、Normal和Small，分别对应：大、中和小三种尺寸

>bold 是否粗体：true|false

>underline 是否下划线：true|false

#### TextAlign(排列控制)

>Left 居左

>Center 居中

>Right 居右

### Table(表格)
### KeyValue(键值)

## 图片类型
### Line(横线)
### Image(图片)
### Barcode(条形码)
### QRcode(二维码)

## 控制类型
### BlankRow(空行)
### CutPage(切纸)
