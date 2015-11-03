title: "iOS——ViewController生命周期"
date: 2014-08-03 16:10:57
tags: viewController生命周期
---

###view的加载

1. ViewController在我们开发中经常用到，它的职责主要包括管理内部各个view的加载显示和卸载，同时负责与其他ViewController的通信和协调。
2. 在iOS中，有两类Viewcontroller，一类是显示内容的，比如UIViewController、UITableViewController等，同时还可以自定义集成自UIViewController的XXXViewController；另一类是ViewController容器，UINavigationController和UITabBarController等，UINavigationViewController是以Stack的形式来存储和管理ViewController，UITabBarController是以Array的形式来管理Viewcontroller。和Android中Activity一样，iOS开发中，ViewController也有自己的生命周期
3. 首先我们来看看View的加载过程，如下图：

	![image](/images/ViewController/1.png)
	
	> 从图中可以看出，在view加载过程中首先会调用loadView方法，在这个方法中主要完成一些关键view的初始化工作，比如UINavigationController和UITabBarController等容器类的ViewController；接下来就是加载View，加载成功后，会接着调用ViewDidLoad方法，`注意:在loadView之前，是没有View的，也就是说，在这之前，view还没有被初始化。`完成viewDidLoad方法后，ViewController里面就成功加载view了，如上图有下角所示。

4. 在Controller中创建view有两种方式，一种是通过代码创建、一种是通过storyboard或interface builder来创建，后者可以比较直观的配置view的外观和属性，storyboard配合iOS6推出的AutoLayout，应该是Apple之后主推的一种UI定制解决方案。

###view的卸载

5. 我们来看一下view是如何被卸载的
	
	![image](/images/ViewController/2.png)

	> 由上图可以看出，当系统给发出内存警告时，会调用didReceiveMemoeryWarning方法，如果当前有能被释放的View，系统会调用viewWillUnload方法来释放view，完成后调用viewDidUnload，至此，view就被卸载了。此时原本指向view的变量要被置为nil，具体操作是在viewDidUnload方法中调用self。XXX = nil；

###小结

6. loadView和viewDidLoad的区别就是，loadView时view还没有生成，viewDidLoad时，view已经生成了，loadView只会被调用一次，viewDidLoad也是被调用一次，当view被添加到其他view中之前，会调用viewWillAppear，之后会调用viewDidAppear。当view从其他view中移除之前，调用viewWillDisAppear，移除之后会调用viewDidDisappear。当view不再使用时，受到内存警告时，ViewController会将view释放并将其指向为nil。
7. ViewController的生命周期中各方法执行流程如下：
init—>loadView—>viewDidLoad—>viewWillApper—>viewDidApper—>viewWillDisapper—>viewDidDisapper—>viewWillUnload->viewDidUnload—>dealloc
 

