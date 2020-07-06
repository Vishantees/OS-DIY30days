# 第1天 从计算机结构到汇编程序入门

## 1 先动手操作

一款[二进制编辑器(Binary Editor)](http://www.vcraft.jp/soft/bz.html), 文中说下载`Bz1621.lzh`但实际上自己下载的是`Bz162.zip`，解压后启动`Bz.exe`即可。

文中说用`!cons_nt.bat`文件启动`CMD`控制台，然后输入`run`即可用QEMU来模拟。
然而本人在WIN10中无法成功操作，发现无法在`CMD`控制台下正常运行，但是`PowerShell`下逐条复制`run.bat`中的每行运行成功。遗憾的是，目前还不会批量执行powershell下的命令，需要现学一下脚本怎么写。

开始制作软盘映像。
- 双击`Bz.exe`，打开文件`hrb27fd0/haribote.img`，复制`0x00-0x90`的数据。
- 新建文件，粘贴。如果无法粘贴则在Edit中把Read Only取消勾选。
- `0x90-0x168000`全部都是0，总共1474560字节(=1440 * 1024 Bytes)，软件最下边有显示当前文件大小。
- 保存为`ddOS.img`，可以自定义名称。

启动QEMU模拟器看下效果吧。
~~这里在仓库的根目录建立了一个文件，作为脚本自动执行一些命令。~~

```cmd
// 原命令的文件夹YourWork是在tolset_h中与z_tools同级
copy helloos.img ../z_tools/qemu/fdimage0.bin
../z_tools/make.exe -C ../z_tools/qemu
```

在当前路径下启动PowerShell后直接复制下面的命令行粘贴运行即可，目前成功启动qemu显示0错误。

```powershell
// 根据自己的练习的文件夹路径，自行调整
copy ddOS.img ../tolset_h/z_tools/qemu/fdimage0.bin
cd ../tolset_h/z_tools/
./make.exe -C qemu
```


## 究竟做了些什么

二进制

## 初次体验汇编程序

作者开发了一个简化的汇编器nask。
对应的汇编语言中DB(define byte)往文件里写入1个字节的指令，
RESB(reserve byte)从当前位置空出来n个字节，以0x00填充。

~~代码没有写，待补充，在附带光盘的projects\01_day\helloos2目录下。~~

原文是启动控制台!cons，输入asm，输入run运行。

当然这里本人自己修改了路径，之后的都是自己调整了路径的，不再单独声明。
**默认笔记中记录的都是根据自己的路径来安排的，并且默认都是在PowerShell下进行的，特此声明。**
**另外，笔记中出现的“作者”表示《30天自制操作系统》的原作者，而“本人”和“笔者”表示写本笔记的人**

```
// 汇编刚刚写好的作者自己开发的汇编
../tolset_h/z_tools/nask.exe ddOS.nas ddOS.img
// 执行和之前一样的命令
copy ddOS.img ../tolset_h/z_tools/qemu/fdimage0.bin
cd ../tolset_h/z_tools/
./make.exe -C qemu
```


## 加工润色

> 再有就是DW指令和DD指令，它们分别是“define word”和“define double-word”的缩写，是DB指令的“堂兄弟”。word的本意是“单词”，但在计算机汇编语言的世界里，word指的是“16位”的意思，也就是2个字节。“double-word”是“32位”的意思，也就是4个字节。
> 对了，差点忘记说RESB 0x1fe-$了。这个美元符号的意思如果不讲，恐怕谁也搞不明白，它是一个变量，可以告诉我们这一行现在的字节数

~~代码没有写，待补充，在附带光盘的projects\01_day\helloos2目录下。~~

汇编中的各部分的说明。
- FAT12格式．..（FAT12 Format）用Windows或MS-DOS格式化出来的软盘就是这种格式。
- 启动区．.........（boot sector）软盘第一个的扇区称为启动区。以0x55AA结尾。
- IPL.........…....initial program loader的缩写。启动程序加载器。名字可以自定义但是必须是八个字节，不足要用空格补齐。
- 启动．.........….（boot）boot这个词是bootstrap的缩写，bootstrap这个词就有“自力更生完成任务”这种意思。这里是把矛盾的操作系统自动启动机制，称为bootstrap方式。


> 启动．.........….（boot）boot这个词本是长靴（boots）的单数形式。它与计算机的启动有什么关系呢？一般应该将启动称为start的。实际上，boot这个词是bootstrap的缩写，原指靴子上附带的便于拿取的靴带。但自从有了《吹牛大王历险记》（德国）这个故事以后，bootstrap这个词就有了“自力更生完成任务”这种意思（大家如果对详情感兴趣，可以在Google上查找，也可以在帮助和支持网页http://hrb.osask.jp上提问）。而且，磁盘上明明装有操作系统，还要说读入操作系统的程序（即IPL）也放在磁盘里，这就像打开宝物箱的钥匙就在宝物箱里一样，是一种矛盾的说法。这种矛盾的操作系统自动启动机制，被称为bootstrap方式。boot这个说法就来源于此。如果是笔者来命名的话，肯定不会用bootstrap这么奇怪的名字，笔者大概会叫它“多级火箭式”吧。

这一小节没有指导运行看看，不过感觉和上一小节的命令一样，可以运行一下看看效果。

