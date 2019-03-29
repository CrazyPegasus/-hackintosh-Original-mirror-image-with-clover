# 集成 CLOVER 引导的黑苹果原版镜像制作教程
准备工作
-------
* macOS环境 (虚拟机实机均可)

* Clover的pkg安装包 (https://github.com/Dids/clover-builder/releases)

* app格式的原版镜像 (可以去App Store下载)

制作过程
-------
1.  新建空白镜像
打开磁盘工具，状态栏‘文件’--‘新建映像’--‘空白映像’，名字任意，不要有中文，大小我们这里设置为6.8G，格式为HFS，即mac os扩展日志式，不要太小，也不建议太大，将其保存至桌面。示例中我做的是10.14的镜像，名字用macOS Mojave 10.14 beta 1 with Clover 4539 18A293u
然后你就发现桌面会多出一个dmg文件和挂载后的一个磁盘。至此空白镜像新建完成

2.  利用命令写入app镜像到空白镜像
打开终端，输入命令来将镜像写进刚才我们新建的那个空盘，命令的大体格式如下：
  
  `sudo createinstallmedia制作工具 --volume 空盘符 –applicationpath app镜像 --nointeraction`

从macOS Mojave开始，--applicationpath参数被抛弃，因此，10.14以后的写入命令为下面的
  
  `sudo createinstallmedia制作工具 --volume 空盘符 --nointeraction`

    由于命令较长，在此稍作说明，方便朋友们理解，以支持更为广泛的命令为例，首先输入sudo后边跟一个空格，然后找到app镜像，右键显示包内容，依次进入/Contents/Resources, 找到一个名为createinstallmedia的文件，将其拖到终端，然后空格输入--volume再空格，然后将磁盘工具新建的那个空白镜像自动挂载后的空磁盘拖进来，空格输入--applicationpath，再空格将整个app镜像拖进来，空格输入--nointeraction, 最后空格回车输入密码再回车，静静等待写入完成即可。
    
    这时你会发现桌面的那个空磁盘现在已经有内容了，而且也有了自己的图标，到这里镜像已经写入完成

集成四叶草引导
-----------
* 写入完成之后，如果你不打算集成四叶草，请略过这一步看下一步。
找到准备好的pkg格式的CLOVER安装包，双击打开
* 按继续
* 再次按继续
* 选择更改安装位置，选择做好的安装盘，然后选择继续
* 选择自定，弹出下面的对话框
* 按照下面勾选的方式进行择选选项，然后选择安装，输入密码
* 安装完成后选择关闭
* 这时我们会发现桌面多出了一个EFI磁盘，这就是我们安装CLOVER后自动挂载的引导盘
* 为了让我们的镜像更加干净，打开桌面上挂载的镜像磁盘，删除里面的EFI-Backups
至此集成CLOVER完成

定制四叶草引导
---------
* 上面我们已经集成好了CLOVER引导，但是现在集成的CLOVER是杂乱的没有意义的，我们需要进行定制，这里我可以将我本机正在用的CLOVER引导定制进去，以达到我使最终镜像成为我本机专用的写入U盘后直接引导可以完美的镜像
* 打开桌面的自动挂载的EFI磁盘，依次打开EFI -> CLOVER
将我自己的引导的必要文件导入进去，有ACPI、kexts、config.plist，然后将一些没用的文件删除以精简我们的镜像，例如drivers64、OEM、kexts下除Other之外的其他文件夹和themes下的除去我们需要的主题之外的其他文件，经过以上所述的处理之后，我的CLOVER定制完成，如下
至此CLOVER定制完成

推出镜像盘
--------
CLOVER定制完成我们需要将镜像磁盘推出以压缩镜像。右键桌面的那两个挂载后的磁盘，选择推出
至此推出镜像盘完成

转换压缩镜像
---------
* 到了最后一步，这时我们的镜像其实已经做完了，只是它个头很大，我们需要将其压缩一下，让它和应用商店下载的app格式的镜像大小相近。打开磁盘工具，状态栏依次选择映像-> 转换
* 弹出的对话框选择我们桌面的那个大镜像，点击选取，然后弹出存储选项的对话框
这里存储为后面的名字和刚开始起的名字一致好了，位置选择文稿，这个看自己心情吧。
* 然后点击转换，然后静静等待，直到完成。
* 转换完成后，点击完成退出磁盘工具，文稿中会出现一个和你当初下载的 app 原版镜像大小差不多的dmg文件，这就是我们最后需要的原版镜像了。
* 将此镜像拿到windows电脑上，用Transmac写进U盘，由于我做的是集成我自己引导专用的CLOBER，所以重启就可以愉快的黑苹果咯。
