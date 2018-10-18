##### 系统文件或目录权限控制命令chmod
chmod 命令用于改变系统文件或目录的访问权限,改命令有两种用法，一种是包含字母和操作符表达式的文字设定法；另一种包含数字设定法。<br>
文件或目录的访问权限分为只读、只写和可执行三种。以文件为例，只读权限表示允许读其内容，而禁止对其做任何的更改操作。可执行权限表示允许该文件作为一个程序执行。文件被创建时，文件所有者自动拥有对改文件的读、写和可执行权限，以便于对文件的阅读和修改。用户也可以根据需要把访问权限设置为需要的任何组合。

有三种不同类型的用户可以对文件或目录进行访问；文件所有者、同组用户、其他用户。所有者一般是文件的创建者。<br>
每一文件或目录的访问权限都有三组，每组用三位表示、分别为文件属性的读、写和执行权限.

ls -al,查看当前目录下的文件权限情况

```
drwxr-xr-x  3 chenliang  staff  136  8 15 15:01 .
drwxr-xr-x  9 chenliang  staff  306  8 15 14:57 ..
drwxr-xr-x  7 chenliang  staff  340  8 15 14:57 .git
-rw-r--r--@ 1 chenliang  staff  111  8 15 15:01 README.md

```

第一个指定了文件类型，在通常意义上，一个目录也是一个文件。如果第一个字符是横线，表示是一个非目录的文件。如果是d,表示是一个目录。后面分别表示了3组用户对当前文件的权限。

```
-rw-r--r--@ 1 chenliang  staff  111  8 15 15:01 README.md
```
为例，表示的是一个文件，所有者有读写权限，同组用户读权限、其他用户读权限。<br>
用户可以利用linux系统提供的chmod命令来重新设定不同的访问权限。也可以利用chown命令来更改某个文件或目录的所有者。利用chgrp来更改用户组。

##### 命令格式:
chmod [-cfvR] [--helo] [--version] model file

##### 命令功能:
用于改变文件或目录的访问权限，用它控制文件或目录的访问权限

##### 命令参数
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<strong>必要参数</strong><br>
-c 当发生改变时，报告处理信息<br>
-f 错误信息不输出<br>
-R 处理指定目录及其子目录下的所有文件<br>
-v 运行时显示详情处理信息<br>

#####  选择参数
--reference=<目录或文件> 设置成具有指定目录或文件具有相同的权限<br>
--version 显示版本信息<br>
<权限范围>+<权限设置> 使用权限范围内的目录或者文件具有指定的权限
<权限范围>-<权限设置> 删除权限范围的目录或者文件的指定权限
<权限范围>=<权限设置> 设置权限范围内的目录或者文件的权限为指定的值<br>
&nbsp;&nbsp;&nbsp;&nbsp;<strong>权限范围</strong> <br>
u:目录或者文件的当前用户<br>
g:目录或者文件的当前群组<br>
0:除了目录或者文件的当前用户或群组之外的用户或群组<br>
a:所有的用户及群组<br>

&nbsp;&nbsp;&nbsp;&nbsp;<strong>权限代号</strong>
r:读权限，用数字4表示<br>
w:写权限，用数字2表示<br>
x:执行权限，用数字1表示<br>
-:删除权限，用数字0表示<br>
s:特殊权限

1).文字设定法：

```
chmod [who] [+|-|=] [mode] 文件名
```

2).数字设定法
chmod [mode] 文件名<br>
若要rwx属性则4+2+1 = 7<br>
若要rw-属性则4+2 = 6<br>
若要r-x属性则4+1 = 7<br>

#### 使用实例

eg1:增加文件所有用户组可执行权限

```
chmod a+x README.md
```
对应的数字指令为

```
chmod 111 README.md
```

```
eg2:chmod ug+w,o-x README.md
```

eg3：对一个目录及其子目录所有文件添加权限 

```
chmod -R u+x test4
```

____


### 2018-08-23
#### SSH相关命令
SSH是一种网络协议，用于计算机之间的加密登录。
ssh只是一种协议，存在多种实现。

##### 基本的用法
SSH主要用于远程登录。

```
ssh user@host
```

该命令为以用户名user，登录远程主机host.
如果本地用户名与远程用户名一致，登录时可以省略用户名

```
ssh host
```

ssh的默认端口是22,登录请求会送进远程主机的22端口。使用p参数，可以修改这个端口。

```
ssh -p 2222 user@host
```

```
$ ssh user@host

　　The authenticity of host 'host (12.18.429.21)' can't be established.

　　RSA key fingerprint is 98:2e:d7:e0:de:9f:ac:67:28:c2:42:2d:37:16:58:4d.

　　Are you sure you want to continue connecting (yes/no)?
```
这个是ssh在第一次登录的时候防止中间人攻击的一种措施，远程主机必须在自己的网站上贴出公匙指纹，以便用户自行核对。

当远程主机的公匙被接受后，它就会被保存在文件$HOME/.ssh/know_host之中。下次再链接这台主机，就会跳过警告

每个SSH用户都有自己的known_hosts文件,此外系统也有一个这样的文件，通常是/etc/ssh/ssh_know_host,保存一些对所有用户都可以信赖的远程主机的公匙。

#### 公匙登录
使用密码登录，每次都必须输入密码，非常麻烦，SSH提供了公匙登录，可以省去输入密码的步骤。

公匙登录：用户将自己的公匙存储到远程主机上，登录的时候，远程主机向用户发送一段随记字符串，用户用自己的私匙加密后，再发回来。远程主机用事先存储的公匙进行解密，如果成功，就证明用户是可信的，直接运行登录shell,不要要求密码。

生成钥匙对

```
ssh-keygen
```

```
ssh-copy-id user@host
```

```
ssh-add privateKey
ssh-add -K privateKey 添加到钥匙串中
```

___

#### mv指令

```
mv指令重命名效果
mv a b

mv移动 :将/a目录移动到/b目录下，并重命名为c
mv /a /b/c
```

#### 环境变量
当要求系统运行一个程序而没有告诉程序的完整路径，系统除了在当前目录下寻找程序外，还应到path中的指定路径去找。

##### 查看环境变量
echo $PATH
export $PATH

```
chendeiMac:jsonsnow.github.io chenliang$ echo $PATH
/Users/chenliang/.rvm/gems/ruby-2.5.1/bin:/Users/chenliang/.rvm/gems/ruby-2.5.1@global/bin:/Users/chenliang/.rvm/rubies/ruby-2.5.1/bin:/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin:/Users/chenliang/.rvm/bi

```

##### 环境变量添加
**临时添加，关闭终端之后就失效**

```
export PATH=/usr/local/webserver/mysql/bin:$PATH
```

**永久有效，对所有用户生效 -修改/etc/profile文件

sudo vim /etc/profile

export PATH="/usr/local/webserver/mysql/bin:$PATH"

执行立即生效语句
```
source /etc/profile
```

**对当前用户生效 - 修改\

```
vim ~/.profile
source ~/.profile
```

环境变量的删除

```
unset PATH
```

#### brew的使用
homebrew,是mac下类似apt-get的软件管理工具

安装brew

```
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

安装软件

```
brew cask install xxx（用来安装一些带界面的应用软件）
eg: brew cask install chrome

brew install xxx
```
卸载软件
brew uninstall xxx


#### Mac 显示任何来源选项命令

```
sudo spctl --master-disable
```


#### Xcode安全性命令
	
遇到问题总是提示输入用户名密码

```
DevToolsSecurity -status
DevToolsSecurity -enable

```
或者  

1. 找到 “钥匙串访问”-> 选中钥匙串栏的“系统”->选中种类栏的"密钥“；

2. 找到类似AppleID对应的文件，就是开发证书对应的密钥，双击，选择”访问控制“，选择"允许所有应用程序访问此项目"，点击”存储更改“即可。

都没有解决我输入用户名密码还是访问不了的问题

**最后解决了，是把在钥匙串中对应在系统的证书移动到登录中去**







