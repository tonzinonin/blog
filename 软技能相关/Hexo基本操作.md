---
### date: 2019-12-15 11:22:37
categories:
 - 博客搭建
tags:
 - hexo
---
#1，hexo常见操作符：

	hexo new "name"       # 新建文章
	hexo new page "name"  # 新建页面
	hexo g                # 生成页面
	hexo d                # 部署
	hexo g -d             # 生成页面并部署
	hexo s                # 本地预览
	hexo clean            # 清除缓存和已生成的静态文件
	hexo help             # 帮助
<!--more-->

#2，Hexo 设置显示文章摘要，首页不显示全文

	Hexo 主页文章列表默认会显示文章全文，浏览时很不方便，可以在文章中插入 <!--more--> 进行分段。

	该代码前面的内容会作为摘要显示，而后面的内容会替换为 “Read More” 隐藏起来。
更多详情请见文章：https://zhuanlan.zhihu.com/p/60578464
