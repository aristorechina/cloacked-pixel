![logo](images/logo.png)

cloacked-pixel
==========

Language: [English](README.md) | **简体中文**

一个平台无关的Python工具，用于实现LSB（最低有效位）图像隐写术和基本检测技术。特性包括：

 - 在插入前加密数据。
 - 在最低有效位中嵌入数据。
 - 提取隐藏的数据。
 - 对图像进行基础分析以检测最低有效位隐写术。

如何使用：

    $ python lsb.py 
    LSB 隐写术。将文件隐藏在图像的最低有效位中。
    
    使用方法:
      lsb.py hide <img_file> <payload_file> <password>
      lsb.py extract <stego_file> <out_file> <password>
      lsb.py analyse <stego_file>

隐藏
----

所有数据在嵌入图片之前都会被加密。加密是必须的。这样做有两个后果：

 - 有效载荷将会稍微大一些。
 - 加密后的有效载荷将具有高熵，并且类似于随机数据。这就是为什么最低有效位中的0和1的频率应该是相同的——0.5。在很多情况下，真实的图像并不具备这一特性，因此我们可以区分未修改的图像和带有嵌入数据的图像。

加密并隐藏一个压缩包：

    $ python lsb.py hide samples/orig.jpg samples/secret.zip p@$5w0rD
    [*] Input image size: 640x425 pixels.
    [*] Usable payload size: 99.61 KB.
    [+] Payload size: 74.636 KB 
    [+] Encrypted payload size: 74.676 KB 
    [+] samples/secret.zip embedded successfully!

原始图像：

![原始图像](images/orig.jpg)

包含75k压缩包的图像：

![嵌入的压缩包](images/stego.jpg)

提取
-------

    $ python lsb.py extract samples/orig.jpg-stego.png out p@$5w0rD 
    [+] Image size: 640x425 pixels.
    [+] Written extracted data to out.
    
    $ file out 
    out: Zip archive data, at least v1.0 to extract

检测
---------

一种简单的检测图像最低有效位是否被篡改的方法基于上述观察——被篡改区域内的最低有效位平均值会接近0.5，因为最低有效位含有加密数据，其结构与随机数据相似。为了分析一张图像，我们将它分割成块，并对每个块计算最低有效位的平均值。要分析一个文件，我们使用以下语法：

    $ python lsb.py analyse <stego_file>

**示例**

![城堡](images/castle.jpg)

现在让我们分析原始图像：

    $ python lsb.py analyse castle.jpg

![原始图像分析](images/analysis-orig.png)

… 现在分析包含我们有效载荷的图像：

    $ python lsb.py analyse castle.jpg-stego.png

![隐写图像分析](images/analysis-stego.png)

注意事项
-----

 - 完全有可能存在最低有效位均值已经非常接近0.5的图像。在这种情况下，这种方法会产生误报。
 - 还存在更复杂的理论方法，主要基于统计学。然而，无法完全消除误报和漏报。

新增
-----
1. README新增中文翻译(虽然是由AI翻译的)
2. 完美支持了Python3
