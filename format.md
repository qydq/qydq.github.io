MacBook安装[mac_install.md]

# 写在前面：

为了一切的爱与被爱理解的不理解，本神只记录小团子芳芳，所需要的动力皆缘于那份想要，你只能好好不被打扰的努力着，希望有一天能再次跟她重逢winter is comming

本备忘录📕：记录当前macbook实用的安装anFramework[an芳芳工具]

本备忘录📕：同步github/qydq.github.io/：对应于mac_install.md 

1. 记录mac下实用工具的安装场景
2. 记录mac下开发工具的安装场景

# 一：记录mac下实用工具的安装场景


## Home-brew安装
Macos软件包管理器

https://brew.sh

## MacTips

>chsh -s /bin/zsh curl -L https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh | sh

更新：upgrade_oh_my_zsh

vi ~/ .zshrc

安装node nom

# brew install node
npm在node v0.6x版本后，内建于node系统

安装doctor (markdown Toc功能)

# nom install doctor -g
# doctoc study.md

Mac命令行解压缩文件安装

# brew install unbar 
# unrar x superlovefangfang.rar


二：记录mac下开发工具的安装场景


Mac安装gradle


Gradle Home 默认的路径如下：

/Applications/Android Studio.app/Contents/gradle/gradle-4.1/bin

需要配置时：

/Applications/Android\ Studio.app/Contents/gradle/gradle-4.1/bin


#配置gradle

1.在/Users/sun/  创建（已存在忽略）.bash_profile

touch .bash_profile

2.打开open -e .bash_profile，输入如下内容

export GRADLE_HOME=/Applications/Android\ Studio.app/Contents/gradle/gradle-4.1

export PATH=${PATH}:${GRADLE_HOME}/bin

3.第二步可以使用VI （m）操作，输入

4.在终端输入如下是配置立即生效

source .bash_profile

5.测试配置gradle是否成功

在终端输入 gradle -v 或者 gradle -versio 则会看到

gradle版本信息

若不成功，请进入gradle主目录/gradle-4.1/bin查看gradle，gradle.bat文件权限，输入如下命令：
ls -al gradle    / ls -al gradle.bat

没有权限则修改权限 输入如下命令修改权限：
chmod 777 gradle   / chmod 777 gradle.bat

6.在第3步的时候可以输入一下命令查看.bash_profile文件内容

cat .bash_profile

/more .bash_profile
空格可以换行

# mac安装Git

如果没有brew 先执行命令。

ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"

或者：

rm -rf /usr/local/.git && ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)

在 sudo vim .bash_profile 加入：

export PATH=/usr/local/bin:$PATH

再更新git

which git

brew update

brew install git

brew install git

Git version


# Mac Finder常用文件地址。。
Android Studio安装目录在    ~/Applications/Android Studio.app
JAVA JDK 默认在Android Studio目录下 ~/Applications/Android Studio.app/Contents/jre/jdk/Contents/Home
JD="/Library/Java/JavaVirtualMachines/jdk1.8.0_131.jdk/Contents/Home"
export JAVA_HOME
SDK 目录 ~/Users/sun/qy/ide/sdk
HOST文件地址 ~/private/etc/hosts
环境变量地址全局修改地址 ~/private/etc/profile

GRADLE HOME :
/Applications/Android Studio.app/Contents/gradle/gradle-4.1/bin




#配置环境变量
比如：配置Java环境变量。。
在终端输入 ：   /usr/libexec/java_home     可以得到JAVA_HOME的路径
安装JDK之后  ： sudo vim /etc/profile   profile是系统配置环境变量的

touch .bash_profile

vim .bash_profile

open -e .bash_profile


需要输入的Java环境变量信息

—---
JAVA_HOME=/Library/Java/JavaVirtualMachines/jdk-9.0.1.jdk/Contents/Home

CLASSPAHT=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar

PATH=$JAVA_HOME/bin:$PATH:

export JAVA_HOME

export CLASSPATH

export PATH

———-

source .bash_profile  这样让环境变量直接生效

java -version

echo $JAVA_HOME

备注1：

system_profiler SPUSBDataType 查看USB连接设备信息

USB 3.0 Bus : iBridge —> Product ID: 0x8600  这个是我的MacBook Pro信息

插上Type C 再插上小米手机以后：

USB 3.0 Bus : USB 2.0 Hub —> Product ID: 0x2817
————————————USB 2.0 Hub : USB Keyboard —> Product ID: 0xd101

Android    —> Product ID: 0xff48

USB 3.0 Bus : iBridge —> Product ID: 0x8600  这个是我的MacBook Pro信息


———————

USB 3.1 Bus:  USB3.0 Hub      Product ID: 0x0817

—————————————USB3.0 Hub  USB 10/100/1000 LAN:  Product ID: 0x8153


备注2：


如果使用的oh my zsh的shell按照以上的配置在关掉terminal之后会失效，这个可真是个坑草了个蛋了，google到了原因及解决办法，确实是zsh的问题，解决办法如下： 

找到User目录下面的.zshrc文件,打开编辑器，在文件的末尾加上
# Enable my profile
source ~/.bash_profile


# Mac基础命令说明
执行命令前使用 . 号表示sh当前进程中执行，如果显示进程已经完成可能包含了exit脚本，进程只能到这里就直接退出了，终端也直接退出了。

可以使用chmod u+x ./xxx.xxx

./xxx.xxx


第一句给当前脚本赋执行权限，第二句让终端窗口的 sh 启动一个新的子进程执行 sh01.sh，这样它退出时就不会影响终端窗口。

如果您对权限不了解，想省掉这一步，也可以直接指定启动一个 bash 进程执行您的脚本：
bash xxx.xxx



#vi编辑器命令，则：Esc 退出编辑模式，输入以下命令：(sudo)

i是输入

:wq  保存后退出vi，若为 :wq! 则为强制储存后退出（常用）

:w    保存但不退出（常用）

:w!   若文件属性为『只读』时，强制写入该档案

:q    离开 vi （常用）

:q!   若曾修改过档案，又不想储存，使用 ! 为强制离开不储存档案。

:e!   将档案还原到最原始的状态！




# Mac 下面Linux常用命令 (sudo#)
移动文件，mv   目标地址 最终地址
查询文件权限 ： ls -(a)l xxx.xxx （文件名）
一共有10位数
其中： 最前面那个 - 代表的是类型
     中间那三个 rw-    代表的是所有者（user）
     然后那三个 rw-    代表的是组群（group）
     最后那三个 r--    代表的是其他人（other）



修改文件权限 sudo chmod 777 xxx.xxx

-rw-------    (600) 只有所有者才有读和写的权限
-rw-r--r--    (644) 只有所有者才有读和写的权限，组群和其他人只有读的权限
-rwx------    (700) 只有所有者才有读，写，执行的权限
-rwxr-xr-x    (755) 只有所有者才有读，写，执行的权限，组群和其他人只有读和执行的权限
-rwx--x--x    (711) 只有所有者才有读，写，执行的权限，组群和其他人只有执行的权限
-rw-rw-rw-    (666) 每个人都有读写的权限
-rwxrwxrwx    (777) 每个人都有读写和执行的权限




# 文件夹下快速索引文件

寻找hosts文件

Command+Shift+G

/private/etc

—

# 文件夹管理自动排序

Crtl+Command+1~7。快捷键调整各种显示选项，我一般喜欢用2，看起来比较方便。


# 浏览器中切换页面

Shift + Command + (上下左右)

Command + 数字


# 切换应用程序

Command + TAB (从左往右）

Shift + Command + TAB (从右往左）



# Android studio下面Mac快捷键

https://www.cnblogs.com/meishan/p/5903477.html

http://blog.csdn.net/zq019/article/details/54618185

http://www.codeceo.com/article/programmer-macbook-workplace.html 高效程序员




# Mac下显示文件的后缀 以及显示隐藏文件

Finder偏好设置 - >高级—> 显示所有文件扩展名


Finder —-前往——电脑  ——>使用Command + F组合键   打开Finder管理器的搜索功能，并在种类栏选择【其它】



显示系统的隐藏文件方法：
在终端上输入：
defaults write com.apple.finder AppleShowAllFiles TRUE; killall Finder

即为显示隐藏文件，如果不要显示系统的这些隐藏文件，修改后面的true为false就好：
defaults write com.apple.finder AppleShowAllFiles FALSE; killall Finder






