title: "UIButton控件设置"
date: 2014-04-22 16:55:43
tags: UIButton
---

1. 创建UIButton对象

	UIButton *bt = [[UIButton alloc]init];
	
2. 隐藏UIButton

	bt.hidden=YES;//此时按钮被隐藏
	bt.hidden=NO;//此时按钮被显示在视图中
   
3. 设置UIButton的坐标和大小

	[bt setFrame:CGRectMake(100, 100, 100, 40)];
	
4. 设置UIButton的颜色

	[bt setBackgroundColor:[UIColor redColor]]
	
5. 设置UIButton的标题字，forState设置的时按钮的当前状态，此时为正常状态

	[bt setTitle:@"aaa" forState:UIControlStateNormal];
	
	UIButton的各种状态
>	正常状态 UIControlStateNormal;
>	高亮状态 UIControlStateHighlighted;
>	禁用状态UIControlStateDisabled；
>	选中状态 UIControlStateSelected;
>	代理状态 UIControlStateApplication;
>	保留状态 UIControlStateReserved;

6. 设置UIButton在正常状态下的字体颜色

	[bt setTitleColor:[UIColor blackColor] forState:UIControlStateNormal];