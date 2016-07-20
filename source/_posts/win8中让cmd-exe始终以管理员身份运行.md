---
title: win8中让cmd.exe始终以管理员身份运行
date: 2016-07-20 14:48:28
tags: [管理员,命令行]
---
### 第一种方式
1. Win+R -- regedit
2. 找到以下位置`HKEY_CURRENT_USER\Software\Microsoft\Windows NT\CurrentVersion\AppCompatFlags\Layers`,新建一个字符串值，命名为`c:\windows\system32\cmd.exe`，一般我们的系统都安装在C盘吧？？？然后右键--修改 -- 数值数据写入“RUNASADMIN”，确定 ！
<!-- more -->
{% asset_img 01.jpg %}

### 第二种方式
如果嫌这样操作麻烦的话就直接复制吧，以系统安装在C盘32位为准：
```bash
Windows Registry Editor Version 5.00

[HKEY_CURRENT_USER\Software\Microsoft\Windows NT\CurrentVersion\AppCompatFlags\Layers]
"c:\\windows\\system32\\cmd.exe"="RUNASADMIN"
```
打开记事本，复制粘贴入以上代码，另存为1.reg，然后双击导入注册表即可。

OK,这下我们Win+R输入cmd，启动时就已经默认是管理员身份了。

{% asset_img 02.jpg %}

### 参考
[win8中让cmd.exe始终以管理员身份运行](http://www.cnblogs.com/hejia/archive/2013/04/20/3032724.html)
