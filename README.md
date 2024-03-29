# ZZXcodeFormat

## 更新后特色
支持OC与swift代码格式化，支持xcode9，10。

## 安装方法

下载ZZXcodeFormat，直接运行one_key_install，so easy。

## one_key_install为您做了什么？

1. 添加`.clang-format`配置文件到个人文件夹`~`，clang-format脚本的规则配置在这里，当然您可以个性化配置，参考[这里](http://clang.llvm.org/docs/ClangFormatStyleOptions.html)
2. 检查并添加Xcode的UUID
3. 编译插件
4. 检查您的gem源，统一修改为最新https://gems.ruby-china.com/
5. 检查update_xcode_plugins是否安装，未安装则安装
6. 执行unsign

执行过后，重启Xcode，如果弹框就选LoadBundle，您就可以在Xcode->Edit栏中看到ZZXcodeFormat了。

## 功能介绍

ZZXcodeFormat包含下面功能：

1. 格式化当前Focus窗口：FocusFile
2. 格式化多个选中文件：SelectFiles
3. 格式化当前选中文本区域：SelectText

## 额外配置
目前我已经为上面三项添加了快捷键i/o/p，辅助键为control+option+command。当然您可以自定义配置快捷键。
例如，为FocusFile添加快捷键：

> 系统设置->键盘->快捷键->应用快捷键->点击添加->应用程序选择Xcode，菜单标题输入FocusFile，键盘快捷键设置shift+command+L.

打开Xcode，点开ZZXcodeForamt，就会发现显示在我们添加的菜单中了。

## 附一张format FocusFile效果
![Focus](https://github.com/V5zhou/ZZClang-format/blob/master/ZZClang-format/FocusFile%E6%A0%BC%E5%BC%8F%E5%8C%96.gif)

---
# 遇到问题总结
## 1.unsign后show in finder失效问题？

最近发现一个bug，就是当执行[unsign](https://github.com/inket/update_xcode_plugins/blob/master/README.md)后，我在新的10.14系统（黑色主题那个）上，执行showinfiner时，无限转圈。

解决方法：

1. 关闭Xcode

2. 打开钥匙串，创建自签名证书： Keychain Access -> 左上角`钥匙串访问` -> 证书助理 -> 创建证书。
![创建证书](https://ws4.sinaimg.cn/large/006tNc79gy1fytjbnp3wkj30y80o8jzh.jpg)

3. 打开terminal使用此证书签名（会等待很长一段时间）：
> sudo codesign -f -s XcodeSigner /Applications/Xcode.app

*XcodeSigner为你刚才的命名*

4. 重签成功后，打开Xcode，编译一下插件的工程文件就可以了。重启Xcode，看到弹框时，允许加载插件。

参考自：https://github.com/XVimProject/XVim/blob/master/INSTALL_Xcode8.md

## 2.`the codesign_allocate helper tool cannot be found or used`重签失败？

解决方法：

查看当前这个命令所在的位置

> locate codesign_allocate

如果提示does not exist，则

> ~~cp /Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/bin/codesign_allocate /usr/bin~~<br>cp命令已失效

原因见：[Operation not permitted](https://www.jianshu.com/p/22b89f19afd6)

那就手动复制，手动打开两个finder窗口，分别前往文件夹/Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/bin/codesign_allocate与/usr/bin，把前者的codesign_allocate拷贝到/usr/bin下。

再执行下面语句是不是不一样了？

> locate codesign_allocate

然后再重新签

> sudo codesign -f -s XcodeSigner /Applications/Xcode.app

等待，等待。。。过会就ok了

参考自：https://www.jianshu.com/p/a62c9efb1e53
