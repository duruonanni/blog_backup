---
title: Hexo站点设置
author: 杜若喃呢
categories:
- 教程
- hexo
tags:
- 教程
date: 2017-05-08 19:25:01
---

## 写在之前的话
本文主要结合我建站时的经历,对配置Hexo的`_config.yml`文件配置说明,以及hexo的常用指令的介绍.   
本文适用于已通过各路Hexo教程完成建站后,需要了解Hexo站点设置的同学.若尚未完成建站,请先行阅读**[todo:建站流程记录][4]**.  
本文内容主要参考了:  
- Setsuna's Blog:[Hexo博客系列][1]  
- 张学志の博客:[hexo配置教程][2]  
- [hexo官方文档][3]  
在此一并感谢~  

由于笔者也是前端与建站的初学者,技浅才疏.如有错误或不足,还望批评指正~

## 一. Blog文件夹详解 
<!--参考:https://hexo.io/zh-cn/docs/setup.html -->
att:  
> - `Blog`文件夹是我的Hexo初始化根目录名称
> - 下面目录中`#`之后的内容是我写的注释,用于解释各个文件及文件夹的作用
### 建站成功后,Blog文件夹内可见如下目录:

```
.
├── _config.yml 	# Hexo的配置文件
├── package.json 	# Hexo版本信息及插件信息
├── .deploy_git 	# 
├── node_modules 	# 
├── public 	# 存放souce文件夹解析出的文件
├── scaffolds 	# 模板文件夹
├── source 	# 资源文件夹
|   ├── _drafts # 存放md格式的草稿文章,默认不发布草稿
|   └── _posts # 存放用于发布的md格式文章
└── themes 	# 主题文件
```

## 二. Hexo的_config.yml配置详解
<!-- 参考1:https://hexo.io/zh-cn/docs/configuration.html -->
<!--参考2:http://blog.csdn.net/xuezhisdc/article/details/53130383 -->
### 配置前的说明:
> * Hexo的_config.yml的储存位置: blog/_config.yml;
> * 在改动配置前,请务必保存一份原始配置,确保出现故障可及时还原;
> * 在配置文件时,冒号后面一定要加一个空格才能识别;
> * 为方便对配置进行二次设置,可使用`#`对配置内容进行注释; 
> * 参考我在下面的注释,可帮助各位理解Hexo中`_config.yml`文件的配置;
 <!--一个句子后加两个空格换行有效,机智 -->

### 我的配置如下  
```
# Hexo Configuration 配置文件
## Docs: https://hexo.io/docs/configuration.html
## Source: https://github.com/hexojs/hexo/

# Site 网站标题
title: 杜若喃呢
# 网站副标题
subtitle: 山中人兮芳杜若
# 网站描述
description: Life is a gift,never take it for granted.
# 网站作者
author: Xiangyu Kong
# 网站语言:需要主题支持
language: zh-Hans
# 时区,默认电脑本地时区
timezone: Asia/Shanghai

# URL 网址
## If your site is put in a subdirectory, set url as 'http://yoursite.com/child' and root as '/child/'
# 网址
url: http://duruonanni.github.io
# 网站根目录.如果网站本身就在根目录下(比如我这样),不管他
root: /
# 网站生成文件格式
permalink: blog/:title/:year:month:day/
permalink_defaults:

# Directory 配置目录名(通常没必要修改)
# 资源文件夹，放在里面的文件会上传到github中
source_dir: source
# 公共文件夹，存放生成的静态文件
public_dir: public
# 标签文件夹，默认是tags.实际存放在source/tags中
tag_dir: tags
# 档案文件夹，默认是archives
archive_dir: archives
# 类别文件夹，默认是categories.实际存放在source/categories中
category_dir: categories
# 代码文件夹，默认是downloads/code
code_dir: downloads/code
# 语言文件夹,默认跟language相同
i18n_dir: :lang
# 不需要渲染的文件,可使用glob表达式来匹配路径。
skip_render: [Readme.md]

# Writing 文章内容
# File name of new posts 新文章名称
new_post_name: :title.md
# 默认布局 除了post外,Hexo默认还支持page和draft布局
default_layout: post
# Transform title into titlecase 把标题转换为 titlecase
titlecase: false
# 新标签打开方式,true是打开一个外部链接
external_link: true
# 转换文件名,值为0不转,1小写,2大写
filename_case: 0
# 显示草稿
render_drafts: false
# 启用 Asset 文件夹
post_asset_folder: false
# 把链接改为与根目录的相对位址
## 默认情况下，Hexo生成的超链接都是绝对地址.通常情况下，建议如此
relative_link: false
# 显示未来的文章
future: true
# 代码高亮
highlight:
  enable: true
  line_number: true
  auto_detect: false
  tab_replace:

# Category & Tag 分类 & 标签
default_category: uncategorized
category_map:
tag_map:

# Date / Time format 日期 / 时间格式 
## Hexo 使用 Moment.js 来解析和显示时间
## Hexo uses Moment.js to parse and display date
## You can customize the date format as defined in
## http://momentjs.com/docs/#/displaying/format/
date_format: YYYY-MM-DD
time_format: HH:mm:ss

# Pagination 分页
# 0则关闭分页功能,全部在1页
## Set per_page to 0 to disable pagination 设置每页显示文章量
per_page: 10
## 分页路径，在public中可以看到
pagination_dir: page

# Extensions 扩展插件
## Plugins: https://hexo.io/plugins/
plugins: 
# 生成RSS的插件
 -hexo-generator-feed
## Themes: https://hexo.io/themes/
# 主题
## Themes: https://hexo.io/themes/
#theme: false #禁用主题
#theme: landscape #默认主题
theme: next

# Deployment
## Docs: https://hexo.io/docs/deployment.html

# 发布
## Docs: https://hexo.io/docs/deployment.html
deploy:
  type: git
  # 部署到github上
  repository: https://github.com/duruonanni/duruonanni.github.io.git
  branch: master
```

## 三. Hexo常用指令
ATT:  
* 以下指令建议在`Git bash`中操作,不建议用CMD;  

### 1. init # 初始化一个网站  
> 语法: 	`hexo init [folder]` 
> 说明: 	`folder` 是网站在本地储存使用的文件夹名,我用的是blog
> ATT:  
 * 一旦配置好_config.yml文件,请不要轻易在blog文件夹使用该命令,这会导致配置文件重置;
 * 在操作中输入folder内容时不用加`[]`符号,下面的所有指令中的`[]`,`<>`也不需要加;
 * 如果需要输入的指令内容(eg: 文章名)包含空格,可用`-`替代或将所有内容用`''`包起来;  
 * 使用此语法前,请先将'Git bash'定位到该用于存储blog文件的文件夹中
 
### 2. generate # 生成网站的静态文件  
> 语法: 	`hexo generate` 
> 说明: 	可简写成 `hexo g`

### 3. new # 新建文章
> 语法: 	`hexo new <title>` 
> 说明: 
* `title`写文章名
* 此语法默认会将新建的文章放到`blog--souce--_posts`文件夹中,你可以在新建的文章中使用Markdown语法编辑内容,最终`_posts`文件夹中的文章会默认以时间顺序发布到你的网站上面.
> ATT: 
* Hexo中文章的文本格式需要是Markdown(后缀名`.md`)
* 关于Markdown的常用语法,可参见我近期的总结[Markdown语法练习][5]
* 这里更推荐以`发表草稿`的方式新建文章,具体方法如下:  
>> - 使用语法: `hexo new draft <title>` 将文章新建到*blog--souce--_drafts*文件夹中,你可以在此处对文章进行编辑.待确认发布时,使用语法:`hexo publish draft <filename>`,可将选定的文章传到`_posts`文件夹.  
>> - 这样的好处是,可以先给文章打好草稿,直接推送写好的成品.可避免未写完的文章放到电脑其他位置弄乱弄丢.(不建议直接在`_posts`文件夹中编辑文章)  

### 4. server # 启动服务器
> 语法: 	`hexo server` 可简写成 `hexo s`
> 说明: 	启动服务器,使站点可通过浏览器,在网址:http://localhost:4000/  预览站点.按**ctrl+c**结束预览
> ATT:
* 启用此语法后,能看到站点中你事先编辑好的内容,此时改变blog文件夹中内容不会在站点内体现,需要退出后再次启动才能看到变动.
* 为实时对站点内容进行调整,可使用语法:`hexo server --debug`进入本地调试模式,并将调试日志写入**debug.log**文件中.此时做出的所有调整都能在网站上动态体现.

### 5. deoloy # 部署网站
> 语法: 	`hexo deploy` 可简写成 `hexo d`
> 说明: 	将站点内容部署到服务器上,使所有人能通过域名访问新发布的文章
> ATT: 	建议使用 'hexo d -g' 语法,在部署之前预先生成静态文件.  


至此:完~🐱‍👤 

![YoYo hakuso](http://storage.live.com/items/AEE68C12565C1619!174679:/YuYu Hakusho.jpg?authkey=AJoh90nl3u6Wj4U)


<!--参考:(不会出现在正文里)-->
[1]: http://www.isetsuna.com/hexo/install-config/ "Hexo博客系列"
[2]: http://blog.csdn.net/xuezhisdc/article/details/53130383 "hexo配置教程"
[3]: https://hexo.io/zh-cn/docs/configuration.html "hexo官方文档"
[4]: https://duruonanni.github.io/blog/%E5%BB%BA%E7%AB%99%E6%B5%81%E7%A8%8B%E8%AE%B0%E5%BD%95/20170509/ "Hexo建站流程"
[5]: https://duruonanni.github.io/blog/Markdown%E8%AF%AD%E6%B3%95%E7%BB%83%E4%B9%A0/20170525/ "Markdown"