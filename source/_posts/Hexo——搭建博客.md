title: "Hexo——搭建博客"
date: 2015-03-10 23:43:59
tags: Hexo——搭建博客
---

###本地搭建hexo

1. 安装nvm(node.js的版本管理工具）
	- cURL：
	
	```
	$ curl https://raw.githubusercontent.com/creationix/nvm/v0.24.1/install.sh | bash
	```

	- Wget:
	
	```
	$ wget -qO- https://raw.githubusercontent.com/creationix/nvm/v0.24.1/install.sh | bash
	```

2. 安装完成后，重启终端并执行下列命令安装Node.js
	```
	$ nvm install 0.10
	```
3. 添加如下内容到.zshrc配置文件
	
	```
	[ -s "/Users/`users`/.nvm/nvm.sh" ] && . "/Users/`users`/.nvm/nvm.sh" # This loads nvm
	```
4. 将nvm的bin目录添加到环境变量中

	```
	以zsh为例：
	1. 打开zsh配置文件
		vi ~/.zshrc
	2. 添加如下内容到.zshrc
		export PATH=$PATH:~/.nvm/v0.10.38/bin
	```
5. 安装hexo

	```
	$ npm install -g hexo-cli
	```
6. 创建hexo文件夹，并安装hexo相关组件
	
	```
	$ hexo init hexo
	$ cd hexo
	$ npm install
	```
7. 本地查看
	输入如下命令后，打开浏览器，并输入[localhost:4000](http://0.0.0.0:4000/)
	```
	$ hexo g
	$ hexo s
	```
8. 部署的时候有时候会失败，可以安装hexo-deployer-git来解决
	```
	$ npm install hexo-deployer-git --save
	```
	
###部署到github

1. 注册github账号

2. 创建github账号同名repository
	
		eg:github账号名为leelovcy，则创建leelovcy.github.io
	
3. 部署到github上
	
	如下所示编辑_config.yml
		
		deploy:
		  type: git
		  repository: http://github.com/LeeLovCY/LeeLovCY.github.io.git
		  branch: master
	`注意：`上述冒号之后有空格
4. 输入如下命令，完成到github得部署，之后打开浏览器并输入<LeeLovCY.github.io>来查看

	```
	$ hexo g
	$ hexo d
	```
	
	