title: "UINavigationController详解与使用（二）页面切换及ToolBar"
date: 2014-04-17 19:45:18
tags: ToolBar
---

####页面跳转

1. RootView跳到SecondView

	首先我们需要新的一个View。新建SecondView，按住Command键然后按N，弹出新建页面，我们新建SecondView
	
	![image](/images/UINavigationVC/10.png)

2. 为Button添加点击事件，实现跳转

	在RootViewController.xib中和RootViewController.h文件建立连接
	![image](/images/UINavigationVC/11.png)

	在RootViewController.m中实现代码，alloc一个SecondViewController，用pushViewController到navigationController中去，并为SecondViewController这时title为secondView.title = @"Second View";默认情况下，title为下个页面返回按钮的名字。
	
	```
	- (IBAction)gotoSecondView:(id)sender {
    SecondViewController *secondView = [[SecondViewController alloc] init];
    [self.navigationController pushViewController:secondView animated:YES];
    secondView.title = @"Second View";
	}
	```

	这是点击GotoSecondView按钮，出现
	![image](/images/UINavigationVC/12.png)

3. 自定义backBarButtonItem

	左上角的返回上级View的barButtonItem的名字是上级目录的Title，如果title或者适合做button的名字，怎么办呢？我们可以自定义
	代码如下：
	
	```
	 UIBarButtonItem *backButton = [[UIBarButtonItem alloc] initWithTitle:@"根视图" style:UIBarButtonItemStyleDone target:nil action:nil];
    self.navigationItem.backBarButton = backButton;
	```
	效果:
	![image](/images/UINavigationVC/13.png)

####ToolBar

1. 显示ToolBar

	在RootViewController.m的`-(void)viewDidLoad`方法中添加代码，这样Toobar就显示出来了
	>[self.navigationController setToolbarHidden:NO animated:YES];

2. 在ToolBar上添加UIBarButtonItem

	新建几个UIBarButtonItem，然后以数组的形式添加到Toolbar中
	
	```
	UIBarButtonItem *one = [[UIBarButtonItem alloc] initWithBarButtonSystemItem:UIBarButtonSystemItemAdd target:nil action:nil];
	UIBarButtonItem *two = [[UIBarButtonItem alloc] initWithBarButtonSystemItem:UIBarButtonSystemItemBookmarks target:nil action:nil];
	UIBarButtonItem *three = [[UIBarButtonItem alloc] initWithBarButtonSystemItem:UIBarButtonSystemItemAction target:nil action:nil];
	UIBarButtonItem *four = [[UIBarButtonItem alloc] initWithBarButtonSystemItem:UIBarButtonSystemItemEdit target:nil action:nil];
	UIBarButtonItem *flexItem = [[UIBarButtonItem alloc] initWithBarButtonSystemItem:UIBarButtonSystemItemFlexibleSpace target:nil action:nil];
	[self setToolbarItems:[NSArray arrayWithObjects:flexItem, one, flexItem, two, flexItem, three, flexItem, four, flexItem, nil]];
	```
	效果如下：
	![image](/images/UINavigationVC/14.png)

	`注意：`用[self.navigationController.toolbar setItems:(NSArray *)animated:YES]这个方法添加Item是不起效果的。下面需要自己动态添加Toolbar时，这个才起效果。

3. 动态添加ToolBar

	我们在SecondView添加动态的ToolBar
	在SecondViewController.h添加
	
	```
	#import <UIKit/UIKit.h>

	@interface SecondViewController : UIViewController
	{
	    UIToolbar *toolBar;
	}
	@end

	```

	在SecondViewController.m添加
	
	```
	- (void)viewDidLoad
	{
	    [super viewDidLoad];
	
	    [self.navigationController  setToolbarHidden:YES animated:YES];
		
	    UIBarButtonItem *addButton = [[UIBarButtonItem alloc] initWithBarButtonSystemItem:UIBarButtonSystemItemSearch target:self action:@selector(gotoThridView:)];
	    toolBar = [[UIToolbar alloc] initWithFrame:CGRectMake(0.0, self.view.frame.size.height - toolBar.frame.size.height - 44.0, self.view.frame.size.width, 44.0)];
	    [toolBar setBarStyle:UIBarStyleDefault];
	    toolBar.autoresizingMask = UIViewAutoresizingFlexibleTopMargin;
	    [toolBar setItems:[NSArray arrayWithObject:addButton]];
	    [self.view addSubview:toolBar];
	     
	    // Do any additional setup after loading the view from its nib.
	}
	```
	以上我觉得移除自带的Toolbar而不是将其隐藏
	[self.navigationController.toolbar removeFromSuperview];
