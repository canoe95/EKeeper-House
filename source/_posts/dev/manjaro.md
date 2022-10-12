---
title: Manjaro Base
date: 2022-08-24 17:53:28
banner_img: /img/maoye.jpg
index_img: /img/mountain.jpg
excerpt: 什么 √8 苦难哲学
tags: Build
categories: Developer
---

> manjaro / win
> 
> rufus 做 manjaro 启动盘

## Start

更换 pacman 源

```bash
sudo pacman-mirrors -c China
```

同步库

```bash
sudo pacman -Syy
```

下载 yay

```bash
sudo pacman -Sy yay
```

输入法

```bash
# 下载vim
yay -S vim

# 下载fcitx
sudo pacman -S fcitx-im
pacman -S fcitx-configtool
pacman -S fcitx-googlepinyin

# 创建fcitx配置文件
vim ~/.xprofile

export GTK_IM_MODULE=fcitx
export QT_IM_MODULE=fcitx
export XMODIFIERS="@im=fcitx"
```

更改目录名为英文

```bash
sudo pacman -S xdg-user-dirs-gtk
export LANG=en_US
xdg-user-dirs-gtk-update

export LANG=zh_CN.UTF-8
```

常用软件下载

```bash
# 下载vscode和gdb
yay -S visual-studio-code-bin
yay -S gdb

# 网易云
yay -S netease-cloud-music

# qq
yay -S deepin-wine-qq

# edge
yay -S microsoft-edge-dev-bin
```

git

```bash
git config --global user.name "name" 设置 git #全局用户名
git config --global user.email "emai@qq.com" #设置 git 全局邮箱
ssh-keygen -t rsa -C "email@qq.com" #生成秘钥

cat /home/northboat/.ssh/id.rsa.pub
```

自动生成合适的 grub,cfg 文件，让 manjaro 读到 win 的引导

```bash
sudo update-grub
```

## Dev

解压现成的 idea 和 dataspell

安装 python 环境

```bash
yay -S anaconda

# 换源
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main
conda config --set show_channel_urls yes

# 加载配置文件
source /opt/anaconda/bin/activate root
```

安装 java 环境

使用 appimage 形式的 qv2ray

## Roco

### Blog

下载 npm / hexo 等

```bash
yay -S nodejs
yay -S npm

npm config set registry https://registry.taobao.org
npm config get registry

sudp npm install cnpm -g
cnpm install -g hexo-cli

# 新建 hexo 项目
hexo init
```

安装主题

```bash
npm install --save hexo-theme-fluid
```

```yml
# 更改 _config.yml 中主题
theme: fluid
```

音乐播放器

```bash
npm install --save hexo-tag-aplayer
```

```yml
# 在 _config.yml 中添加
aplayer:
  meting: true
```

```bash
# 新增页面
hexo new page album

# /album/index.md 的 type 需要设置为 playlist
```

播放器格式，其中 xxxx 为网易云网页上的 url 的 id，单首歌设为 song，歌单设为 playlist

```markdown
{% meting "xxxx" "netease" "song" "theme:#181c27" "mutex:true" "listmaxheight:340px" "preload:auto" %}

{% meting "xxxx" "netease" "playlist" "theme:#181c27" "mutex:true" "listmaxheight:340px" "preload:auto" %}
```

### Docs

下载 vuepress

```bash
sudp npm install vue -g
sudo npm install vuepress -g
```

新建 vuepress 项目

```bash
npm init
```

更改 package.json，添加 scripts

```json
"scripts": {
  "test": "echo \"Error: no test specified\" && exit 1",
  "docs:dev": "vuepress dev docs",
  "docs:build": "vuepress build docs"
},
```

更换主题颜色

在 .vuepress 目录下新建 styles 文件夹，新增文件 palette.styl

```stylus
// 主题颜色
//#9999FF 蓝紫       
//#6667AB 长春花蓝
//#6F6695 淡紫
//#361F3B 暗紫
//#6667AB 深蓝
$accentColor = #6667AB
$textColor = #2c3e50                        // 文本颜色
$borderColor = #eaecef                      // 边框线颜色
$codeBgColor = #282c34                      // 代码块背景色
$backgroundColor = #ffffff                  // 悬浮块背景色


//示例修改相关样式f12找到需要修改的地方找到对应class类拿过来直接用就行了
.sidebar-group.is-sub-group > .sidebar-heading:not(.clickable){
  opacity: 1
}

//.navbar > .links{
  //#FFC8B4 橙色
  //#F7FED5 浅蓝黄
  //#DDD0C8 灰
  //#CAD8D8 朝白
  //background: #CAD8D8
//}

//.navbar{
  //background: #CAD8D8
//}
```
