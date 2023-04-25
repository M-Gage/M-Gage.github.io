---
title: Hexo + GitHub Pages搭建个人博客，超级简单好上手
date: 2023-04-12 17:10:15
categories:
- 学习
tags: 
- Hexo
- Git

----
## 前言

闲来没事搞个0成本的博客写写心得

## 准备

* 安装Git
* 安装Nodejs
* GitHub 账号
* 灵巧的手，带点编程的脑

## Hexo

先去 [Hexo官网](https://hexo.io/zh-cn) 直接按照下方的命令按顺序在无脑复制执行

```
npm install hexo-cli -g
hexo init blog
cd blog
npm install
hexo server
```

顺利的话，你就看到下面的结果，不顺利你就放弃吧

{% img class /images/20230412/img.png 750 500 '"" "运行成功提示"' %}

点击本地连接到浏览器，然后你就拥有了一个本地的博客，酷！

## 提交代码

在当前目录下 */blog* 创建 *.gitignore* 文件，将以下内容复制进去

```
.DS_Store
Thumbs.db
db.json
*.log
node_modules/
public/
.deploy*/
_multiconfig.yml

```

以上完成后就可以提交代码啦  
如果提交代码顺利，那么GitHub仓库里的文件夹结构应该跟下面一样

{% img class /images/20230412/img2.png 750 500 '"" "文件夹结构示例"' %}

## GitHub workflows

在当前GitHub仓库中的 *.github/workflows* 文件下创建 *pages.yml*
将下面的配置写入

```
name: Pages

on:
  push:
    branches:
      - main # 默认分支

jobs:
  pages:
    runs-on: ubuntu-latest
    permissions:
      contents: write
    steps:
      - uses: actions/checkout@v2
      - name: Use Node.js 16.x # 改成你安装的版本
        uses: actions/setup-node@v2
        with:
          node-version: "16"  # 改成你安装的版本
      - name: Cache NPM dependencies
        uses: actions/cache@v2
        with:
          path: node_modules
          key: ${{ runner.OS }}-npm-cache
          restore-keys: |
            ${{ runner.OS }}-npm-cache
      - name: Install Dependencies
        run: npm install
      - name: Build
        run: npm run build
      - name: Deploy
        uses: peaceiris/actions-gh-pages@v3
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./public
          
```

将上面的Nodejs版本改成你安装的版本  
使用命令查看安装的版本，例如当前版本是： 18.0.13 那么你只要把上面的16改成18就可以了

``` 
node --version 
```

## 一键部署

1. 安装 *hexo-deployer-git* 插件 执行下面的命令安装插件

``` 安装命令
npm install hexo-deployer-git -s   
```

如果 *package.json* 文件里面有 hexo-deployer-git 就可以了 {% img class /images/20230412/img4.png 750
500 '"" ""' %}

2. 远程部署配置

在 *_config.yml* 中添加以下配置

``` 
deploy:
  type: 'git'
  repo: https://github.com/M-Gage/m-gage.github.io.git # 这里复制你的GitHub仓库地址
  branch: gh-pages
```

3. 执行部署命令

``` 
hexo clean && hexo deploy
```

骚等一段时间后

{% img class /images/20230412/img3.png 750 500 '"" ""' %}

看到以上内容说明部署成功了

4. 配置GitHub Pages  
   在GitHub仓库页面中前往 Settings > Pages > Source，并将 branch 改为 gh-pages  
   {% img class /images/20230412/img5.png 750 500 '"" ""' %}  
   那就赶紧看看 <GitHub 用户名>.github.io 这个网址能不能正常运行吧

## 结语
  这一套简单也好上手，Hexo 还支持很多好的主题，又有中文文档，GitHub Pages 省掉域名和服务器，只是访问可能慢一点而已  
另外写好之后直接在本地执行命令就可以部署，省事省心  
下次在说一下换主题和开始写作


[](/Users/Gage/hexo/blog/themes/ayer/source/images/video_PV03_Music.mp4)