title: "iOS--Pch文件的使用"
date: 2014-09-08 14:13:53
tags: .Pch文件
---

###Xcode6添加pch文件

`前言：`Xcode6中不在为开发者自动创建pch文件，在pch文件中我们可以添加一些琐碎的宏定义，在项目中任何地方都可以引用，加快了编译的速度

1. Xcode6之后的版本都是需要自己添加的,步骤如下：

	![image](/images/pch/1.png)
	
2. 随后需要进行一些配置

	![image](/images/pch/2.png)
	
	在上图中的第三步Prefix Header的值进行设置时最好使用
	`$(SRCROOT)/工程名/pch文件名`
	相对路径的书写方法，不要写成绝对路径，如果使用绝对路径会出现一些问题。