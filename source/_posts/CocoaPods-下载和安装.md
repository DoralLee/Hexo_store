title: "CocoaPods 下载和安装"
date: 2015-05-25 15:02:45
tags: CocoaPods
---

###下载和安装

	在安装之前，首先要在本地安装好Ruby环境。如果你在本地已经安装好Ruby环境，那么下载和安装CocoaPods将十分简单，只需要一行命令。在终端中输入如下命令
	```
	sudo gem install cocoapods
	```
	但是，如果你在天朝，在终端中敲入这个命令之后，会发现半天没有任何反应。原因无他，因为那堵墙阻挡了cocoapods.org。我们可以用淘宝的Ruby镜像来访问cocoapods。按照下面的顺序在终端中依次敲入命令：
	```
	gem source --remove https://rubygems.org/
	gem source -a http://ruby.taobao.org/
	```
	为了验证你的Ruby镜像是并且仅是taobao，可以用一下命令查看：
	```
	gem source -l
	```
	只有在终端中出现下面文字才表明你上面的命令是成功的：
	```
	*** CURRENT SOURCES ***
	http://ruby.taobao.org/
	```
	此时，你再次在终端中运行：
	```
	sudo gem install cocoapods
	```
	
###使用CocoaPods

	安装好之后就是如何使用它了。我们以AFNetworking来说明吧
	AFNetworking类库在Github地址是:(https://github.com/AFNetworking/AFNetworking)
	为了确定AFnetworking是否支持Cocoapods，可以用Cocoapods的搜索功能验证一下。在终端中输入：
	```
	pod search AFnetworking
	```
	随后你会看到相关信息，准备工作也差不多了，下面让我们进入正题
	首先先利用Xcode创建一个项目工程。随后在终端中进入所创建的项目文件中，然后运行：
	```
	vim init
	```
	随后cocoapods会为项目自动生成一个Podfile文件，执行：
	```
	vim Podfile
	```
	然后再Podfile文件中target和end之间输入以下信息：
	```
	platform :ios, '7.0'
	pod "AFNetworking", "~> 2.0"
	```
	随后退出编辑模式，执行：
	```
	pod install
	```
	不过你在执行此条命令时会要等N久，主要是执行上述命令时会升级Cocoapods的spec仓库，加一个参数就可以省略这一步，然后速度就会有相当大的提升，命令如下：
	```
	pod install --verbose --no-repo-update
	pod update --verbose --no-repo-update
	```
	
	