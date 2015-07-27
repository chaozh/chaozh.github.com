title: Win7 X64下PHP开发环境配置
date: 2014-12-17 11:04:58
---

还记得本科时节花费半个学期配置PHP开发环境，现在轻松就能自己完全配置搞定了，稍微总结记录一下。

## 1.安装Apache

配置Apache2.4.9(httpd-2.4.10-win64-VC11.zip )
解压下载的安装包:httpd-2.4.10-win64-VC11.zip 将其放到自己的安装目录(我的目录E:\Apache24)
然后对http.conf(E:\Apache24\conf\http.conf)配置文件进行修改-使用记事本打开就行

(1)修改ServerRoot Apache的根路径:
ServerRoot "c:/Apache24"改成=&gt;ServerRoot  "E:/Apache24"

(2)修改ServerName你的主机名称:
ServerNamewww.example.com:80将前面的#去掉,该属性在从命令行启动Apache时需要用到。

(3)修改DocumentRoot Apache访问的主文件夹目录,就是php、html代码文件的位置。Apache默认的路径是在htdocs(E:\Apache24\htdocs)下面,里面会有个简单的入口文件index.html。这个路径可以自己进行修改,我这里将其配置在我自己新建的文件夹www(E:\www)下。
DocumentRoot "c:/Apache24/htdocs"
&lt;Directory"c:/Apache24/htdocs"&gt;
改为
DocumentRoot "E:\php\www"
&lt;Directory "E:\php\www"&gt;

(4)修改入口文件配置: DirectoryIndex一般情况下我们都是以index.php、index.html、index.htm作为web项目的入口。Apache默认的入口只有index.html需要添加其他两个的支持，当然这个入口文件的设置可以根据自己的需要增减,如果要求比较严格的话可以只写一个index.php,这样在项目里面的入口就只能是index.php
DirectoryIndex index.html

改为

DirectoryIndex index.php index.htm index.html

(5)设定serverscript的目录:
ScriptAlias/cgi-bin/ "c:/Apache24/cgi-bin/"改为 ScriptAlias/cgi-bin/ "e:/Apache24/cgi-bin"

&lt;Directory"c:/Apache24/cgi-bin"&gt;
AllowOverride None
Options None
Require all granted

改为
&lt;Directory"e:/Apache24/cgi-bin"&gt;
AllowOverride None
Options None
Require all granted

(6)尝试启动Apache
开始---运行,输入cmd,打开命令提示符。接着进入e:\Apache24\bin目录下回车httpd回车，
没有报错的话就可以测试了（保持该命令窗口为打开的状态）。
把Apache24\htdocs目录下的index.html放到e:\php\www目录下，用浏览器访问会出现“It works”那么就说明apache已经正确安装并启动了。也可以自己写一个简单的index.html文件也可以打开。
(7)将Apache加入到window服务启动项里面并设置成开机启动
先关闭httpd的服务（将命令窗口关闭即可）
重新打开一个新的命令窗口进入到E:\Apache24\bin目录下，注意这里必须以管理员身份运行
添加HTTP服务的命令是：httpd.exe -kinstall -n "servicename" servicename是服务的名称，我添加的是：httpd.exe -k install -n "Apache24"命令成功后会有成功的提示，此时你可以在window服务启动项中看到Apache24这个服务
如果要卸载这个服务的话，先要停止这个服务，然后输入httpd.exe -k uninstall -n "Apache24"卸载这个服务。
当然也可以通过E:\Apache24\bin下面的ApacheMonitor.exe来启动Apache这里就不多说了
如此Apache的配置就基本完成了。

## 2.安装PHP

配置php5.5.19（php-5.5.19-Win32-VC11-x64.zip）

(1)、将下载的php-5.5.19-Win32-VC11-x64.zip 解压到安装目录下我的是（E:\php）

(2)、将目录下的php.ini-development文件复制一份并改名为php.ini他是php的配置文件

(3)、为Apache服务添加php支持
打开Apache的配置文件http.conf在最后加上
# php5 support
LoadModule php5_module e:/php/php5apache2_4.dll
AddType application/x-httpd-php .php .html .htm
在LoadModule下面添加
# configure thepath to php.ini
PHPIniDir "e:/php"
即读取php配置文件位于你的php根目录，就再不需要傻逼的把php.ini放到系统windows目录下面了。

(4). 设置ext目录，并去掉相应的externsion设置，比如php_mbstring和php_mysql

(5).重启Apache服务器。

(6).测试。删除www中其他文件，新建一个index.php，内容为&lt;?php phpinfo(); ?&gt;保存，访问出现php的信息就说明php已经成功安装。

先前使用的是php5.6.3版本，发现这是个比较激进的版本，于是就切换为5.5系列。特别要注意的是把php.ini中的short_open_tag改为On，这样才能解析“&lt;?”这种简略形式

## 3.安装MySQL

配置安装mysql-5.6.22-winx64.zip
1\. 配置mysql的MYSQL_HOME和PATH等环境变量，方便后面使用命令行启动服务

2\. 在解压得到的目录下面配置my.ini文件，内容如下
[mysqld]
loose-default-character-set = utf8
basedir = E:/mysql-5.6.11-winx64
datadir = E:/mysql-5.6.11-winx64/data
port = 3306
sql_mode=NO_ENGINE_SUBSTITUTION,STRICT_TRANS_TABLES
character_set_server = utf8

[client]
loose-default-character-set = utf8
打开命令提示符，进入%MYSQL_HOME%/bin目录，执行命令：mysqld -install将mysql安装到windows的服务。执行成功后会提示：E:\mysql-5.5.10-win32\bin&gt;Service successfully installed.
如果想要卸载服务执行命令：mysqld -remove。
然后在命令提示符下执行：net start mysql就能启动mysql了，停止服务输入命令：net stop mysql。如果想设置mysql是否自动启动，可以在开始菜单-&gt;运行中输入service.msc打开服务管理进行设置。

3\. 修改root用户密码
mysql -u root mysql
mysql&gt; update mysql.user set password=PASSWORD('root') where User='root' ;
mysql&gt; flush privileges ;