# 最新版Notepad++设置中文不起作用

> ​	该工具作者反华，官网上都一清二楚，但是咱还是能分清的类似的软件还有很多，都可以替代Notepad，所以工具应该是被作者修改代码,此教程给想用工具的人，将这一问题解决，所以需要找到被修改的代码，将代码修改回去。

## 打开Notepad设置文件

`chineseSimplified.xml`

<img src="http://image.codehuan.cn/image/image-20210127155626505.png" alt="image-20210127155626505" style="zoom:50%;" />

找到第852行代码

<img src="http://image.codehuan.cn/image/image-20210127160012789.png" alt="image-20210127160012789" style="zoom:50%;" />

将</Scintillas> 修改为 </MarginsBorderEdge>

<img src="http://image.codehuan.cn/image/image-20210127160117875.png" alt="image-20210127160117875" style="zoom:50%;" />

启动软件，重新设置为中文即可。

<img src="http://image.codehuan.cn/image/image-20210127160233281.png" alt="image-20210127160233281" style="zoom: 33%;" />