# Why I gave up using windows (Chinese only)

## 前言

[Microsoft Windows](https://en.wikipedia.org/wiki/Microsoft_Windows) 是巨硬公司开发的操作系统（废话）。
作者作为一名前Windows用户，在过去近20年的时间内一直在与Windows的各种bug（feature？）抗争，以至于最后彻底放弃该开发平台，
仅将其留作游戏和平台兼容性测试使用。
本文将对这作者使用Windows曾遇到过的坑进行总结，以便他人作出决策。

每个具体控诉条目的前面有一个复选框，表示该问题至今有没有得到完美的解决。

## 巨硬大坑：缺少完整的命令行解决方案

作为开发平台，命令行的重要性无需多言。
没有完整的命令行解决方案带来的痛苦是非常巨大的。
这里的命令行既包括shell也包括terminal。
如果你不清楚两者的区别，那么请参阅[这个答案](https://unix.stackexchange.com/questions/4126/what-is-the-exact-difference-between-a-terminal-a-shell-a-tty-and-a-con)。
简单来说，terminal是黑框框/蓝框框，shell是黑框框/蓝框框里面运行的东西。
Windows的命令行解决方案的缺失主要体现在一大一小两个方面——
一大方面是没有足够好用的shell，
一小方面是自带terminal却又极其难用。
下面具体来介绍。

### 关于terminal的控诉

- [ ] Windows对命令行的东西存在一个最最根本的限制：每一个可执行文件要么是CLI（命令行）程序，要么是GUI（图形界面）程序。
    这个区分是内蕴的：在程序被创建出来的时候它的性质就已经固定了。
    - [ ] 这样做的后果是，一些CLI/GUI两用的程序必须做成GUI类型（否则无法启动窗体）；
        然而为了从命令行查看它的帮助，执行`file.exe --help`命令只能得到一个又一个的对话框，其中把本该在命令行中的帮助一一列出，经常需要好几个对话框才足够。
- [ ] Windows与Linux不同：对于后者，terminal和shell可以做到完全分家，也即任意terminal搭配任意shell皆可正常工作。然而Windows却做不到这一点。
    - [ ] 最常见的两个shell是`cmd.exe`和`powershell.exe`。它们都自带有terminal，没有办法更改terminal。
- [ ] 对于这两个东西自带的terminal，修改任何简单到诸如更换字体/调整字号等等的基本设置都是**异常困难**的——
    - [ ] 请问，如何（在不Google的情况下）更换PowerShell窗口的字体（并保留修改，对以后打开的每个窗口都应用同样设置）？
    - [ ] 请问，如何（在不Google的情况下）将PowerShell窗口的背景色从蓝色改成深灰色，将亮黄色改成亮橘色，将亮红色改成粉红色，并保留修改？
- [ ] 由于作者在绝大部分时间都在使用PowerShell自带的terminal，这里附带几条关于它的控诉：
    - [ ] 当你在terminal里面按下Ctrl+C的时候，
        terminal究竟是会将其传递给shell（后者将把Ctrl+C理解成发送SIGINT信号中断当前程序运行）呢，
        还是会将其理解成复制屏幕上选取了的文本呢，是取决于当前有没有选中文本的。
        这样做的一个非常严重后果是可能会意外终止当前程序执行。
    - [ ] 与上一条类似，当你按下Ctrl+V的时候，terminal会将剪贴板的内容传递给shell。
        然而如果剪贴板的内容包含多行文字，最后一行以外的所有内容将会被**立即执行**，无需你按下回车键。
        这意味着如果你不小心选中了一段包含`rm -r -fo ~`的文字……
    - [ ] 既然terminal决定了Ctrl+V是粘贴，那么请问如何在PowerShell里面输入一个Tab字符？一个回车字符？
        （注：Linux系的Terminal可以先按Ctrl+V再按Tab来输入特殊字符）
    - [ ] 当你选中了一段文本，terminal会立即向shell发送类似于XOFF的flow control character以暂停输出，以便你安心选择文本。
        然而悲催的是，你有时候只是不小心在terminal里面点了一下，程序输出缓冲区就很快完全填满，
        也就只能**暂停运行**以等待输出缓冲区清空了。
        于是你就等吧……等到宇宙末日你的程序也不会执行完毕的。
    - [ ] 当你运行一个需要你盯着每一行输出结果看的程序的时候，如果你看的速度没有程序输出的速度快，
        那么一个很自然的想法是稍微向上滚动一点，宁可暂时跟不上进度也要看清楚每一行输出。
        然而terminal根本不会给你这样做的希望——你虽然可以向上滚动，但是一旦程序输出哪怕是一个字节，
        你的滚动条也会瞬间移动到最底端，**强迫**你观察最新的输出。
        当然，聪明的你也可以用上面那条的方法暂停程序的输出以便查看日志，不过万一你忘了恢复执行呢？
    - [ ] 哦对了，请问，以上设置如何修改？比如我想用Ctrl+Shift+C/V来复制粘贴，而Ctrl+C/V原封不动发送给shell？
        我不想在程序输出的时候自动跳转到最下方又该怎么办？
- [ ] 当然，你还可以使用各种层出不穷的第三方terminal，或者官方的[Windows Terminal](https://github.com/microsoft/terminal)。
    不过性能是个问题。（作者对此不是非常确定，不过连terminal这种基础设施都需要第三方的东西可还行？）
- [ ] 除了两个最知名的shell——`cmd.exe`和`powershell.exe`——各自带的terminal以外，还有其他一些shell，比如`Git Bash`、`msys2`、`cygwin`等等。
    - [ ] 请问，如何改变这些shell默认的terminal？
        注意我说的是改变它们匹配的terminal，也就是说双击这个shell的时候应该打开哪个terminal，
        不是在说已经打开某一个包含多个tab的terminal以后如何启动这个shell。
        后面这个情况在第三方terminal中已经实现地很好了。
- [ ] 请问，如何在普通terminal中运行管理员权限的shell，或者反过来？

### 关于shell的控诉

## 巨硬中坑：没有合理的软件生态系统

## 巨硬小坑：完全不存在的系统支持

