<!-- #freeze -->
[FrontPage](../FrontPage.md)

# What's this?
- [Google App Engine](http://code.google.com/appengine) runs on [Pandaboard](http://pandaboard.org/)(with [Linaro/Ubuntu](http://www.linaro.org/downloads/)) x 6 .
![Structure.png](pandacloud_attachment_Structure.png)

### Materials
- Presentation material
    - [On slideshare](http://www.slideshare.net/noritsuna/panda-cloud)
- Video
    - [On YouTube](http://www.youtube.com/watch?v=pSGNf0KZ71c)
- Twitter
    - [Linaro](https://twitter.com/#!/LinaroConnect/status/132182476641669120)
- Blog
    - [Linaro Blog](http://www.linaro.org/linaro-blog/2011/11/05/connect-videos/)

<br>

<br>

<br>

- Pictures<br>
![server_front.jpg](pandacloud_attachment_server_front.jpg)
![server_side.jpg](pandacloud_attachment_server_side.jpg)<br>
![codereview.png](pandacloud_attachment_codereview.png)
<br>

# Why?
- This is a very powersaving server!
    - The powersaving is the most important issue for server.<br>
![powersaving.png](pandacloud_attachment_powersaving.png)

# Using hardwares & softwares
### Hardwares
- [Pandaboard x 6](http://pandaboard.org/)
    - [Linaro/Ubuntu](http://www.linaro.org/downloads/)

### Middleware
- [Google App Engine](http://code.google.com/appengine)
    - Sample GAE Application
        - [Rietveld](http://code.google.com/intl/ja/appengine/articles/rietveld.html)
        - Code review system(ITS) for GAE
    - Datastore Backend
        - [MySQL](http://www.mysql.com)
    - Memory cache
        - [Memcached](http://memcached.org)
    - Task Queue / Messaging
        - [RabbitMQ](http://www.rabbitmq.com)
        - [Ejabberd](http://www.process-one.net/en/ejabberd)
    - HTTP Server via FastCGI
        - [NGINX](http://nginx.net/)
        - [Apache2](http://httpd.apache.org/)
        - [FastCGI](http://www.fastcgi.com)
    - Supervisor
        - [Supervisor](http://supervisord.org)
    - Load balancer
        - Apache2�s load balancer

# How to setup
1. Setup Linaro/Ubuntu
    1. Please look at [Linaro's homepage](http://www.linaro.org/downloads/).
1. Install softwares needed  by [TyhoonAE](http://code.google.com/p/typhoonae/) ((work on Linaro/Ubuntu for Pandaboard))
    - You have to install as **NOT** root user.
        - OpenJDK 6
        - MySQL
        - Python
        - Erlang
        - Apache2 & proxy
        - gettext
        - xsltproc
        - expat1
    1. apt-get commands:
```
$ sudo apt-get update
$ sudo apt-get install openjdk-6-jdk mysql-server python-mysqldb libmysql++-dev libncurses5-dev libssl-dev python-dev python-setuptools libexpat1-dev gettext xsltproc erlang erlang-nox erlang-dev erlang-src apache2 libapache2-mod-proxy-html
```
1. Build [TyhoonAE](http://code.google.com/p/typhoonae/)
    - You have to install as **NOT** root user.
    1. Download [typhoonae-buildout-0.2.0](http://typhoonae.googlecode.com/files/typhoonae-buildout-0.2.0.tar.gz)
    1. Unzip to **/home/linaro/tyhoonae**
    1. Edit 'buildout.cfg"
        1. pcre's URL is wrong. Change to "http://vps.googlecode.com/files/pcre-8.02.tar.gz"
    1. Exec following commands:
```
$ sudo python bootstrap.py
$ ./bin/buildout
```
        - If you have an error & stop command, you shuold try following things:
        1. Stop "generate_app" part
        - Edit "./parts/rabbitmq/Makefile"<br>
Change line:
```
From:
TARGETS=$(EBIN_DIR)/rabbit.app $(INCLUDE_DIR)/rabbit_framing.hrl $(BEAM_TARGETS) 
To:
TARGETS=$(INCLUDE_DIR)/rabbit_framing.hrl $(BEAM_TARGETS)
```
Delete line:
```
$(EBIN_DIR)/rabbit.app: $(EBIN_DIR)/rabbit_app.in $(BEAM_TARGETS) generate_app
	escript generate_app $(EBIN_DIR) $@ < $<
```
        1. Stop "checking erl" part
    - Edit "./eggs/rod.recipe.ejabberd-1.1.3-py2.7.egg/rod/recipe/ejabberd/__init__.py"<br>
Delete line:
```
retcode = subprocess.call(cmd)
if retcode != 0:
	raise Exception("building ejabberd failed")
```
    - Change files:
```
wget http://www.noritsuna.com/download/buildout.cfg
wget http://www.noritsuna.com/download/ejabberd-2.1.5.tar.gz
cp ejabberd-2.1.5.tar.gz ./downloads/
```
        1. If it stops other part, you shuold try again "./bin/buildout".
1. Setup configs
    1. [TyhoonAE](http://code.google.com/p/typhoonae/)
        1. Change "typhoonae.cfg"
```
datastore = mysql
```
 
```
mysql_db = mysql
mysql_host = [MySQL server's IP Address]
mysql_passwd = [MySQL root's password]
mysql_user = root
```
    1. MySQL
        1. Change "/etc/mysql/my.ini" ((This setting has some security issues. If you want to provide this server to Internet, you should research about the following things!!!))<br>
Delete line:
```
bind-address = 127.0.0.1
```
        1. Change grant connection limit
```
mysql -u root -p
grant all on *.* to root@"[your network address].%" identified by '[root password]' with grant option;
```
Ex.
```
grant all on *.* to root@"192.168.1.%" identified by 'pass' with grant option;
```
        1. If you can't connet to MySQL, you have to change MySQL's lib for Python.<br>
Add to "./eggs/MySQL_python-1.2.3c1-py2.7-linux-armv7l.egg/MySQLdb/connections.py"'s line 188:
```
kwargs2['client_flag'] = client_flag
kwargs2['db'] = 'mysql'
kwargs2['host'] = '[MySQL server's IP address]'
kwargs2['user'] = 'root'
kwargs2['passwd'] = '[MySQL root password]'
```
    1. Apache
        1. Enable some mobules:
```
LoadModule proxy_module modules/mod_proxy.so 
LoadModule proxy_balancer_module modules/mod_proxy_balancer.so 
LoadModule proxy_connect_module modules/mod_proxy_connect.so 
LoadModule proxy_ftp_module modules/mod_proxy_ftp.so 
LoadModule proxy_http_module modules/mod_proxy_http.so 
LoadModule rewrite_module modules/mod_rewrite.so
```
        1. Edit "/etc/apache2/sites-available/001-default"<br>
Add following lines:
```
ProxyRequests Off
ProxyPass / balancer://cluster/ lbmethod=byrequests
ProxyPassReverse / balancer://cluster/
```
 
```
<Proxy *> 
Order Deny,Allow 
Allow from all 
</Proxy> 
<Proxy balancer://cluster/>
BalancerMember http://[Panda server's IP address]/ loadfactor=10
BalancerMember http://[Panda server's IP address]/ loadfactor=10
BalancerMember http://[Panda server's IP address]/ loadfactor=10
</Proxy>
```
### Install GAE application
- Ex.Rietveld
    - Get Rietveld for panda
```
wget http://www.noritsuna.com/download/rietveld-panda.tar.gz
```
    - Install Rietveld
```
cd [TyphoonAE dir]
./bin/apptool  --datastore=mysql [Rietveld unziped dir]/rietveld
```
### Manage GAE server
- Start
```
./bin/supervisord
```
- Shutdown
```
./bin/supervisorctl shutdown
```
- Check status
```
./bin/supervisorctl
```

# Info
### Contact us
- info
    - info atmark siprop.org
- Noritsuna
    - noritsuna atmark siprop.org
