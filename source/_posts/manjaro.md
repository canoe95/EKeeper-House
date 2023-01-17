---
title: Manjaro
date: 2022-08-24 17:53:28
banner_img: /img/moon.jpg
index_img: /img/buddhism.jpeg
excerpt: 什么 √8 苦难哲学
tags: Developer
categories: Study
---

> rufus 制作启动盘安装 manjaro
> 

## 半小时搭建学习环境

### System

initPacman.sh：初始化包管理，更换 pacman 源，同步库，下载 yay

```bash
sudo pacman-mirrors -c China
sudo pacman -Syy
sudo pacman -Sy yay
```

自动生成合适的 grub.cfg 文件，让 manjaro 读到 win 的引导

```bash
sudo update-grub
```

修改 grub 设置

```bash
# 查看
sudo cat /etc/default/grub

GRUB_DEFAULT=saved
GRUB_TIMEOUT=5
GRUB_TIMEOUT_STYLE=hidden
GRUB_DISTRIBUTOR="Manjaro"
GRUB_CMDLINE_LINUX_DEFAULT="quiet apparmor=1 security=apparmor resume=UUID=75c1c3b5-d413-4ca6-8946-15a0fc7ef18b udev.log_priority=3"
GRUB_CMDLINE_LINUX=""

# 删除 quiet 加上 loglever=5
GRUB_CMDLINE_LINUX_DEFAULT="apparmor=1 security=apparmor resume=UUID=75c1c3b5-d413-4ca6-8946-15a0fc7ef18b udev.log_priority=3 loglever=5"

# 使 grub.cfg 文件生效
sudo grub-mkconfig -o /boot/grub/grub.cfg
# 这里要找一下 grub.cfg 的位置，可以在 /boot/efi/ 里
```

校正时间

```bash
timedatectl set-local-rtc 1 --adjust-system-clock
timedatectl set-ntp 0
```

brightness.sh：亮度调节

```bash
echo "------start."
read -p "Enter the bright_lever: " lever
case $lever in
  1) let bright=9000;;
  2) let bright=15000;;
  3) let bright=20000;;
  4) let bright=25000;;
esac
sudo su << EOF # 后续为子进程或子 shell 的输入
cd /sys/class/backlight/intel_backlight/
echo $bright > brightness
EOF
echo "--------end."
```

面板排布

<img src="/img/panel.png">

### Chinese

changeInput.sh：中文输入法

```bash
# 下载vim
yay -S vim

# 下载fcitx
sudo pacman -S fcitx-im
pacman -S fcitx-configtool
pacman -S fcitx-googlepinyin

# 创建fcitx配置文件
vim ~/.xprofile

# 文件填写以下内容
export GTK_IM_MODULE=fcitx
export QT_IM_MODULE=fcitx
export XMODIFIERS="@im=fcitx"
```

changeDir.sh：更改目录名为英文，因为安装时选择的中文系统，默认 home 下的目录名将是中文

```bash
sudo pacman -S xdg-user-dirs-gtk
export LANG=en_US
xdg-user-dirs-gtk-update

export LANG=zh_CN.UTF-8
```

### Software

installSoftware.sh：常用软件下载

vscode

```bash
# vscode和gdb
yay -S visual-studio-code-bin
yay -S gdb
```

休闲

```bash
yay -S netease-cloud-music # 网易云
yay -S microsoft-edge-dev-bin # edge
```

qq

```bash
yay -S deepin-wine-qq # wine版
# electron 新版，腾讯，我真的哭死
yay -S linuxqq-new
```

- Typora 解压使用
- 使用 AppImage 版本的度盘

官网下载 natapp 用于内网穿透：[natapp](https://natapp.cn/)

- 执行命令为

```bash
./natapp -authtoken=xxxxx
```

### Developer

initGit.sh：初始化 git

```bash
git config --global user.name "NorthBoat" 设置 git #全局用户名
git config --global user.email "northboat@163.com" #设置 git 全局邮箱
ssh-keygen -t rsa -C "northboat@163.com" #生成秘钥

cat /home/northboat/.ssh/id.rsa.pub
```

installAnaconda.sh：配置 Python 环境

```bash
yay -S anaconda

# 换源
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main
conda config --set show_channel_urls yes

# 加载配置文件
source /opt/anaconda/bin/activate root
```

- 解压 dataspell

installJava.sh：配置 Java 环境

```bash
yay -S jdk8-openjdk
yay -S jdk11-openjdk

archlinux-java status # 查看 java 版本
archlinux-java set java-11-openjdk # 切换默认java版本
```

- 解压 IDEA，学生帐号激活
- 解压 Maven，手动建 repo 文件夹装包

installMysql.sh

```bash
yay -S mysql

# 初始化MySQL，记住输出的root密码
mysqld --initialize --user=mysql --basedir=/usr --datadir=/var/lib/mysql
# 设置开机启动MySQL服务
systemctl enable mysqld.service
systemctl daemon-reload
systemctl start mysqld.service
# 使用MySQL前必须修改root密码，MySQL 8.0.15不能使用set password修改密码
mysql -u root -p
mysql> ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY '新密码';

# 图形化界面
yay -S mysql-workbench
```

installRedis.sh：安装 Redis，解压后进入目录

```bash
yay -S gcc g++
yay -S make
yay -S pkg-config

# 编译
make && make install

# 新建文件夹并将配置文件复制到此处
cd /usr/local/bin
mkdir config
cp /home/northboat/tool/redis-6.2.6/redis.conf config
vim config/redis.config

# 通过配置文件启动 redis-server
./redis-server config/redis.conf
```

- 使用 another-redis-desktop-manager 的 AppImage 版本作为 redis 可视化工具

installRabbitMQ.sh：下载 rabbitmq

```bash
yay -S rabbitmq rabbitmqadmin

# 启动管理模块
sudo rabbitmq-plugins enable rabbitmq_management
# 启动
sudo rabbitmq-server
```

- 管理界面默认端口：15672
- 客户端默认端口：5672

magic.sh：科学上网

```bash
yay -S v2ray # 下载 v2ray 内核
```

- 使用 appimage 形式的 qv2ray
- 保存有祖传版内核，在 qv2ray 首选项中配置使用

### Repo

blog.sh：下载 npm、hexo、vuepress 等以及插件

```bash
yay -S nodejs
yay -S npm

npm config set registry https://registry.taobao.org
npm config get registry

sudo npm install cnpm -g
cnpm install -g hexo-cli

# hexo
npm install --save hexo-theme-fluid # 主题
npm install --save hexo-tag-aplayer # 播放器

# vuepress
sudo npm install vue -g
sudo npm install vuepress -g

npm install @vuepress-reco/vuepress-plugin-bgm-player --save # 播放器
```

pull.sh：拉取仓库脚本

```bash
echo "-------start"
cd Blog
git pull
echo -e "blog pull end!\n" # -e 启用转义字符
cd ..
cd Docs
git pull
echo -e "docs pull end!\n"
cd ..
cd Index
git pull
echo -e "index pull end!\n"
echo "---------end"
```

