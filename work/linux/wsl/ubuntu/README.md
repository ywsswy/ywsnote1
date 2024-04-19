# Top things to do after installing ubuntu.

## -1. Ubuntu系统 sh默认连接的是dash，这会造成某些命令找不到例如pushd
执行sudo dpkg-reconfigure dash 命令，将dash设置为No

## 0. <modify /etc/apt/sources.list(optional)>

sources.list file_format
deb<#binary>/deb-src <ftp_address> <version_name> <限定词>

## 1. sudo apt update

update package-list according to the sources.list

## 2. sudo apt upgrade

update installed package

## 3. others（install/update some package）

sudo apt install vim<or vim-gnome>

<sudo rm python;sudo ln -s python3 python>  
sudo apt install python3-pip  
sudo pip3 install flask  

sudo apt install chromium-broswer
<install extension vimium for chromium>
sudo apt install pepperflashplugin-nonfree

system setting -> language
sudo apt install ibus-pinyin
reboot
system setting -> text entry -> add ibus(pinyin) -> preference
reboot

sudo add-apt-repository ppa:wine/wine-builds
sudo apt update
sudo apt install winehq-devel
tar xvf wineQQ9.0.3_23729.tar.xz -C ~/
<chinease messy code>http://www.wuwenhui.cn/2692.html

<software>
<simplescreenrecorder>https://jingyan.baidu.com/article/14bd256e6ef04cbb6c26124f.html
setting:output profile:YouTube;Save as;mkv;
sudo apt install aegisub

# supplement

## apt & dpkg

apt install <packagename>
dpkg -i <packagename.deb>
the former will auto solve the dependence while the latter not

## apt & apt-get

apt contains apt-get apt-cache apt-config

## apt search <packagename> |grep <packagename>

search all

## apt list --installed |grep <packagename>

search installed

## use apt-get purge rather than remove

## apt-get install use more space than apt-get purge clean space

because auto install dependence, while purge won't remove dependence

## sudo add-apt-repository ppa:<user>/<ppa-name>

ppa是个人软件包档案Personal Package Archives，个人用户为其他用户提供apt源下载和更新，添加的会放在sources.list.d中，密钥会放在trusted.gpg.d中

## sudo apt show

view software information


## others

superTuxkart
steam Don't starve
PPPoE上网 设置隐藏网络共享wifi，见书签
翻墙
挂载windows系统sudo mount /dev/sda5 /media/hill/Local disk
