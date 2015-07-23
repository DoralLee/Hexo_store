title: "SVN常用命令集合"
date: 2014-07-04 09:16:36
tags: SVN
---

1. SVN常用命令集

	> svn info 查询svn根目录的信息，这个命令当你想知道某个svn目录是从哪个服务器上下来的时候特别有效
	> svn add 增加一个文件到根目录里，注意要用svn commit上传这个修改
	> svn status 查询当前目录下文件修改的情况，a表示增加，M表示修改
	> svn diff 查看本目录下所有的文件有哪些区别，当然可以指定到文件名
	> svn commit -m "fix bug" file上传某个文件的修改，并增加注释
	> svn ci 上传所有的修改，会提示你添加修改记录
	> svn log file 查询某个文件的修改记录
	> svn checkout 从svn服务器上去一个目录，带svn信息
 	> svn export 从svn服务器上取出一个目录，仅源文件，没有讨厌的.svn信息
 	> svn revert 回滚本地所有的未上传的所有修改，慎用，回复该本地所有的修改操作。可一次回滚一个目录或者文件
 	> svn revert file --depth=infinity 回滚该目录下的所有文件
 	> svn diff -r3 rules.txt 将本地的working目录下的文件和服务器的r3版本之间进行比较
 	> svn diff -r 3:2 rules.txt 比较服务器上得r2版本和r3版本	