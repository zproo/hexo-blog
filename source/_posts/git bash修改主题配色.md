---
title: git bash修改主题配色
comment: true
date: 2017-05-05 14:30:20
tags: [Git bash, Git, Scheme,Shell]
cover: http://ortur5wom.bkt.clouddn.com/cover-bash.jpg
---

# 1.获取主题配置文件

在git bash窗口点击右键，选择option选项。

![11ScreenClip](http://ortur5wom.bkt.clouddn.com/bash1.png)

进入`Options`窗口，可以看到looks标签下的`Theme`选择。

![22ScreenClip](http://ortur5wom.bkt.clouddn.com/bash2.png)

但是git bash默认的是没有主题供选择的。我们点击`Theme`下的`Color Schema Designer`按钮，此时我们会来到下面这个网站。

你可以自定义喜欢的背景、文字为你喜欢的搭配，然后点击右上角的`Get Scheme`，这里我们选择`.minttyrc`类型

![3ScreenClip](http://ortur5wom.bkt.clouddn.com/bash3.png)

![44ScreenClip](http://ortur5wom.bkt.clouddn.com/bash4.png)

此时你会获得如下的文件内容：

![55ScreenClip](http://ortur5wom.bkt.clouddn.com/bash5.png)

下面给出文字版：

``` shell
BackgroundColour=13,25,38
ForegroundColour=217,230,242
CursorColour=217,230,242
Black=0,0,0
BoldBlack=38,38,38
Red=184,122,122
BoldRed=219,189,189
Green=122,184,122
BoldGreen=189,219,189
Yellow=184,184,122
BoldYellow=219,219,189
Blue=122,122,184
BoldBlue=189,189,219
Magenta=184,122,184
BoldMagenta=219,189,219
Cyan=122,184,184
BoldCyan=189,219,219
White=217,217,217
BoldWhite=255,255,255
```

（这是上面网站默认的主题配置，你可以直接使用）

# 2.设置Git Bash主题

好的，我们有了主题配置文件，现在我们来设置为Git Bash的主题。

1. 打开git bash命令行（任意位置），输入如下命令,回车

```shell
cd ~
vim .minttyrc
```

2. 进入vim编辑模式后，我们先按`Insert`键，即切换到“插入”状态。就可以通过上下左右移动光标，或空格、退格及回车等进行编辑内容了，和WINDOWS是一样的。我们删除原有内容，将刚才得到的`主题配置文件`复制到此处。
  ![66ScreenClip](http://ortur5wom.bkt.clouddn.com/bash6.png)
3. 按键盘左上角的"ESC"，左下角的插入状态消失。 然后这时，我们输入“冒号”，即":"(不需双引号)，在下方会出现冒号，等待输入命令。再输入"x"，即保存退出。（注意：在英文状态下进行输入）

此时，重启Git Bash,你会发现主题配置已经生效！！



