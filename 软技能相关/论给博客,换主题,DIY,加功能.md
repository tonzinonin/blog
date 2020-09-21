---
### 2019-12-15 13:56:35
categories: 博客搭建
tags: [Git语法,hexo]
---
主题下载并启用
<!--more-->

	进入命令行，下载 NexT 主题，输入：

	Copy
	git clone https://github.com/theme-next/hexo-theme-next themes/next

	修改站点配置文件 _config.yml，找到如下代码：

	Copy
	#Themes: https://hexo.io/themes/
	theme: landscape

	将 landscape 修改为 next 即可。
修改默认背景

	通过theme->landscape->source->css->image
	把要换的图片替换成文件夹下的banner图片并更名
	在public文件夹下用git bash执行hexo generate。然后hexo s预览即可。

索引：

	论如何加网站加标签页，以及给文章分类加标签：
	https://blog.csdn.net/ganzhilin520/article/details/79047249
	https://blog.csdn.net/Com_ma/article/details/76039859
	NexT官方文档：
	http://theme-next.iissnan.com/getting-started.html

