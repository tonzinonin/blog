---
### 2019-12-15 10:40:39
categories:
 - Git
tags:
 - Git语法
---
 I 上传文件

以下步骤详细输入
<!--more-->
git(包括ssh等)
	Ctrl+ins  复制
   	Shift+ins 粘贴

	1,git init
	2,git add .
		//初始化命令
	3,git config --global user.mail "264533xxxx"
	4,git config --global user.name "tonzinonin"
		//对于3-5之间的命令，如果之前已经给git绑定了github的帐号，可以不写
	5,git commit -m "commit name"
	6,git remote add origin https://github.com/tonzinonin/tonzinonin-code
		//你要上传的库的地址
	8,git push -u origin master
		-->if step 8 error Enter：
		[git pull --rebase origin master
		[git push origin master

II delete远程文件

	1, git pull origin master 将远程仓库里面的项目拉下来
	2, dir  查看有哪些文件夹
	3, git rm -r --cached target  删除target文件夹
	4, git commit -m '删除了target'  提交,添加操作说明

解决nmp下载慢终极方案(cmd,git,node.js通用)：

npm config set registry https://registry.npm.taobao.org
(所谓的淘宝镜像)

III 使用vscode进行git操作
1，打开相应的文件夹
2，源代码管理-》加号
3，随便输入几个指令
4，建立github链接，把推送的链接导向库，第一次要先在浏览器打开github主页才有效
5，等待后点击3个点的按钮，然后找到"推送到"并且点击
6，之后到github查看，检查是否将文件上传到库

VI Git和CMD一样都是ctrl+insert复制，shift+insert粘贴

V vscode进行源代码同步云端：
	tips:源代码管理
	M modified
	你已经在github中添加过该文件，然后你对这个文件进行了修改，就会文件后标记M
	U untracked
	你在本地新建了这个文件，还未提交到github上，就会标记U
	D delete
	你删除了这个文件，vscode-git会记录下这个状态
	6,U
	表示有6个错误且untracked(未跟踪)
执行III步骤后，相当于把库连接到了文件夹，之后你对文件夹内部的每一次修改都会在源代码管理出现。
之后再次推送就行了。

6 vscode+git 上传unity源代码。
在github新建库，gitignore勾选unity。

可选：在unity设置：Edit->Project Settings->Editor
将序列化模式改成Force Text以后.unity .mat .prefab 什么的 就是文本格式了,默认是二进制的.
总体来说 多人合作的开发 推荐改为 Force Text


