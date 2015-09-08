title: "手写代码UIButton图片和文字位置的设置"
date: 2015-08-12 13:39:25
tags: 手写Button设置
---

1. 首先对button的初始化，以及图片和文字的设置

	```
	//添加放弃和留下按钮
    UIButton *dropBtn = [UIButton buttonWithType:UIButtonTypeCustom];
    dropBtn.frame = CGRectMake(1*WIDHT/5, getViewHeight(foodNameLabel)+50, 60, 60);
    [dropBtn setImage:[UIImage imageNamed:@"icon_No@2x.png"] forState:UIControlStateNormal];

    dropBtn.titleLabel.font = [UIFont systemFontOfSize:15];
    [dropBtn setTitleColor:[UIColor blackColor] forState:UIControlStateNormal];
    [dropBtn setTitle:@"放弃" forState:UIControlStateNormal];
    dropBtn.imageEdgeInsets = UIEdgeInsetsMake(0, -15, 0, 0);
    dropBtn.titleEdgeInsets = UIEdgeInsetsMake( 0, -5, 0, 0);
    [dropBtn addTarget:self action:@selector(dropAction:) forControlEvents:UIControlEventTouchUpInside];
    
    [self.view addSubview:dropBtn];
    
    UIButton *keepBtn = [UIButton buttonWithType:UIButtonTypeCustom];
    keepBtn.frame = CGRectMake(WIDHT - 1*WIDHT/5 - 60, getViewHeight(foodNameLabel)+50, 60, 60);
    [keepBtn setImage:[UIImage imageNamed:@"icon_OK@2x.png"] forState:UIControlStateNormal];
    
    keepBtn.titleLabel.font = [UIFont systemFontOfSize:15];
    [keepBtn setTitleColor:[UIColor blackColor] forState:UIControlStateNormal];
    [keepBtn setTitle:@"留下" forState:UIControlStateNormal];
    keepBtn.imageEdgeInsets = UIEdgeInsetsMake(0, 35, 0, -15);
    keepBtn.titleEdgeInsets = UIEdgeInsetsMake( 0, -65, 0, 0);
    [keepBtn addTarget:self action:@selector(keepAction:) forControlEvents:UIControlEventTouchUpInside];
    
    [self.view addSubview:keepBtn];

	```
	
2. 以上就是所有代码，其中下列代码是对图片和文字位置的设置，UIEdgeInsets属性就上左下右的顺序设置，`‘-’加到数字前就是指负向偏移，‘+’是正向偏移`，其实imageEdgeInsets中的第一个数值是对图片对于button上边距的距离，第二个数值是对图片对于button左边距的距离，第三个数值是对图片对于button下边距的距离，第四个数值是对图片对于button的右边距的距离，而titleEdgeInsets的设置类似的。
	```
	keepBtn.imageEdgeInsets = UIEdgeInsetsMake(0, 35, 0, -15);
	keepBtn.titleEdgeInsets = UIEdgeInsetsMake( 0, -65, 0, 0);
	```
	我所实现的效果图如下：
	![image](/images/手写UIButton/20150812.png)
	具体需要实现什么效果还是需要你自己把握的