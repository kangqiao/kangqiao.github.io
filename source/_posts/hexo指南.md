---
title: hexo指南
date: 2017-01-26 16:06:58
tags: [hexo]
comments: true
---

### GitHub + Hexo
GitHub Pages是GitHub提供的一项免费服务.每个GitHub账号可以申请一个pages仓库用来存放网页文件.而GitHub在已经安装好了服务器程序以便于浏览器访问这些网页.由于GitHub Pages不支持php和数据库,因此只能在上面部署静态博客框架.
Hexo也是现在使用比较广的,也比较简单部署的静态框架.所以我们选择GitHub pages + Hexo来部署我们的博客.

### 使用hexo搭建部署Github博客

#### 初始化博客
```
// 使用npm全局安装Hexo
npm install -g hexo-cli
// 初始化博客工程 安装依赖
npm init <folder>
cd <folder>
npm install
// 安装hexo git提交插件
npm install hexo-deployer-git --save
```
#### 配置博客`_config.yml`
```
title: 这里填写博客的标题
subtitle: 这里填写博客的副标题
description: 这里填写博客的描述
author: 这里填写博客的作者
language: 这里填写博客的语言,如果是中文填写”zh-CN”
timezone: Asia/Shanghai
url: ”http://kangqiao.github.io“

post_asset_folder: true 
#当您设置post_asset_folder为true参数后，在建立文件时，Hexo会自动建立一个与文章同名的文件夹，您可以把与该文章相关的所有资源都放到那个文件夹，如此一来，您便可以更方便的使用资源。
deploy:
  type: 这里填写”git”
  repo: 写”git@github.com:kangqiao/kangqiao.github.io.git”
  branch: master
```
_ 如果需要了解更多hexo的配置或者想要做更高级的定制,可以查看[官方配置说明](https://hexo.io/docs/configuration.html) _

#### 发表一篇文章
在终端命令行输入`hexo new 文章标题`
我们可以在本地博客文件夹`source`->`_post`文件夹下看到我们新建的markdown文件。

```
---
title: hexo指南
date: 2017-07-26 16:06:58
tags: [hexo]
---

你好，欢迎来到我的个人技术博客。
![图片](./图片名.png)
```

保存后，我们进行本地发布 `$ hexo server`
![本地发布](./hexo_server.png)

#### 发布博客到GitHub Pages
我们只要在终端执行这样的命令即可：
```
$ hexo g -d
这个命令的意思是使用hexo生成整个博客的网页文件,并且上传到我们刚才repo里面填写的git仓库里.hexo会自动检索我们博客文章的改动,删除,增加,并生成一套新的网页.
```
或者:
```
$ hexo generate
$ hexo deploy
```

### 上传博客工程

上一步部署博客到Github以后，我们可以在Github仓库的master分支上看到我们上传的博客文件。
但是这个博客文件是不包含hexo配置的，所以我们需要新建分支，使用git指令将带hexo配置的Github工程文件上传到新建的分支上。
![github创建分支](./github_create_branch.png)
在本地博客根目录下使用git指令上传项目到Github:
```
// git初始化
git init
// 添加仓库地址
git remote add origin https://github.com/用户名/仓库名.git
// 新建分支并切换到新建的分支
git checkout -b 分支名
// 添加所有本地文件到git
git add .
// git提交
git commit -m ""
// 文件推送到hexo分支
git push origin hexo
```
其他设备上clone下Github上新建的分支的文件到本地
在另一台设备上使用git指令下载Github新建分支上的文件:
```
// 克隆文件到本地
git clone -b 分支名 https://github.com/用户名/仓库名.git
```
本地写文章
在`source`->`_posts`文件夹下新建md文件，并编辑好保存后：

同步项目源文件到Github
```
// 添加源文件
git add .
// git提交
git commit -m ""
// 先拉原来Github分支上的源文件到本地，进行合并
git pull origin 分支名hexo
// 比较解决前后版本冲突后，push源文件到Github的分支
git push origin 分支名hexo
```

### 增加评论功能`gitment`

[Hexo博客框架下Gitment取代多说评论](https://zonghongyan.github.io/2017/06/29/201706292034/)

### 参考
- [https://hexo.io/zh-cn/](https://hexo.io/zh-cn/)
- [如何使用github和Hexo搭建个人博客](https://shikieiki.github.io/2017/02/23/%E5%A6%82%E4%BD%95%E4%BD%BF%E7%94%A8gitHub%E5%92%8CHexo%E6%90%AD%E5%BB%BA%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2/)
- [多设备同步hexo搭建的Github博客](http://www.jianshu.com/p/6fb0b287f950)









