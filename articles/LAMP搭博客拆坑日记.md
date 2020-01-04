2019/7/18 17:12:06 
# Ubuntu LAMP + wordpress 搭建blog踩坑日記

本人犯賤，開始抱著鍛煉自己能力的決心去，自行搭建環境，結果落得。。。
在此記下犯賤過程。

## 安裝 LAMP 环境
LAMP 是 Linux、Apache、MySQL 和 PHP 的缩写，是 Wordpress 系统依赖的基础运行环境。
(centos 可以去搭建LNMP)

### 安装 Apache2
在终端输入该命令 ，使用 apt-get 安装 Apache2： 
sudo apt-get install apache2-y 
安装好后，在浏览器输入http://服务器地址/，看到 “it works” 界面，说明 apache2 安装成功。

### 安装 PHP 组件
apt-get 里有 php7.0 ，所以我们可以直接安装 php7.0 ：  
sudo apt-get install php7.0 -y 	
安装 php 相关组件： 	
sudo apt-get install libapache2-mod-php7.0	

### 安装 MySQL 服务
安装 MySQL 过程中，控制台会提示您输入 MySQL 的密码，您需要输入两次密码，并记住您输入的密码，后续步骤需要用到：  
sudo apt-get install mysql-server -y  
安装 php MySQL相关组件： 
sudo apt-get install php7.0-mysql

### 安装 phpmyadmin
使用 apt-get 安装 phpmyadmin，安装过程中，您需要根据提示选择 apache2 ，再输入root密码 和数据库密码			
sudo apt-get install phpmyadmin -y 	
建立 /var/www/html 下的软连接： 	
sudo ln -s /usr/share/phpmyadmin /var/www/html/phpmyadmin 	
重启 MySQL 服务 	
sudo service mysql restart 	
重启 Apache 服务： 	
sudo systemctl restart apache2.service  	
浏览器输入http://服务器地址/phpmyadmin，输入数据库账户密码即可进行数据库操作。

### 安装并配置 Wordpress
安装 Wordpress	
我们需要下载一个 Wordpress 压缩包： 	
wget https://cn.wordpress.org/wordpress-4.7.4-zh_CN.zip （也可以下在官网最新的但是是英文的，源也比较慢）	
下载完成后，解压这个压缩包 	
sudo unzip wordpress-4.7.4-zh_CN.zip 	
解压完后，就能在 Wordpress 文件夹里看到 Wordpress 的源码了 

### 为 wordpress 配置一个数据库
进入 mysql，输入以下代码后，按提示输入您MySQL密码: 	
mysql -u root -p 	
为 wordpress 创建一个叫 wordpress 的数据库： 	
CREATE DATABASE wordpress; 	
为 这个数据库设置一个用户为 wordpressuser：	 
CREATE USER wordpressuser; 	
为这个用户配置一个密码为 password123： 	
SET PASSWORD FOR wordpressuser= PASSWORD("password123"); 	
为这个用户配置数据库的访问权限： 	
GRANT ALL PRIVILEGES ON wordpress.* TO wordpressuser IDENTIFIED BY"password123"; 	
生效这些配置 	
FLUSH PRIVILEGES; 	
然后退出 mysql 	
exit;	

### 配置 wordpress
由于PHP默认访问 /var/www/html/ 文件夹，所以我们需要把 wordpress 文件夹里的文件都复制到 /var/www/html/ 文件夹 	
sudo mv wordpress/* /var/www/html/ 	
修改一下 /var/www/html/ 目录权限： 	
sudo chmod -R 777 /var/www/html/**注：千万别忘记改权限，不然可掉头发了。** 	
将apache指定到index.html 	
sudo mv /var/www/html/index.html /var/www/html/index~.html 	
重启 Apache 服务： 	
sudo systemctl restart apache2.service 	
测试访问	
输入http://ip地址	
进入word press安装

## 安装完成后的各种问题

### wordpress安装主题或插件需要FTP问题

当时我安装完，啥主题都安装不了，安装啥，啥失败，简直xxx想爆粗。后来我直接主题文件下载下来，用ftp上传到服务器，虽说可行，但总觉得白璧微瑕，后来查了很多资料。。在下面分享给大家，希望对大家有用。

当安装完成后，可能会有一些人发现，安装主题或安装插件时，都需要填写ftp账号和密码，而且还安装失败。

提供下面方法	
**1、**用ftp连接到服务器（我用的filezila）进入 wp-content 目录，新建tmp文件夹，设置文件夹的权限为777		
cd 到 wp-content 目录	
mkdir tmp	
chmod 777 文件夹(设置权限)

**2、**设置wp-content目录中的plugins（插件）和themes（主题）文件夹权限为777

（建议递归把var www html wordpress和wp-content都全设置为777）

**3、**在wordpress目录下找到wp-config.php文件，并将其下载到本地打开

**4、**修改wp-config.php

/** WordPress目录的绝对路径。 */

if ( !defined('ABSPATH') )

define('ABSPATH', dirname(__FILE__) . '/');

后面添加如下代码

define('WP_TEMP_DIR', ABSPATH.'wp-content/tmp');
 
define("FS_METHOD", "direct");
 
define("FS_CHMOD_DIR", 0777);
 
**5、**将wp-config.php文件上传并覆盖原文件

### 不能发布文章，提示发布失败

上面步骤成功后，就可以在panel安装plugin了，搜索 Classic Editor 安装即可正常发布文章

### 发布文章后在实际网站，找不到此文章

原因是路径含有中文

编辑文章，在标题下方会有一个url，点击修改路径的中文字符(至于换成什么自己发挥把，也没多大影响)