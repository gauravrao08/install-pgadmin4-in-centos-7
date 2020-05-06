# install-pgadmin4-in-centos-7
How to install pgadmin4 on centos 7 for postgress 11

yum -y install https://download.postgresql.org/pub/repos/yum/12/redhat/rhel-7-x86_64/pgdg-redhat-repo-latest.noarch.rpm

yum -y install pgadmin4

mv /etc/httpd/conf.d/pgadmin4.conf.sample /etc/httpd/conf.d/pgadmin4.conf

vi /etc/httpd/conf.d/pgadmin4.conf

```
<VirtualHost *:80>
#ServerName girnar.pgadmin.com
ServerName IP-server-IP
LoadModule wsgi_module modules/pgadmin4-python3-mod_wsgi.so
WSGIDaemonProcess pgadmin processes=1 threads=25
WSGIScriptAlias /pgadmin4 /usr/lib/python3.6/site-packages/pgadmin4-web/pgAdmin4.wsgi

<Directory /usr/lib/python3.6/site-packages/pgadmin4-web/>
        WSGIProcessGroup pgadmin
        WSGIApplicationGroup %{GLOBAL}
        <IfModule mod_authz_core.c>
                # Apache 2.4
                Require all granted
        </IfModule>
        <IfModule !mod_authz_core.c>
                # Apache 2.2
                Order Deny,Allow
                Deny from All
                Allow from 127.0.0.1
                Allow from ::1
        </IfModule>
</Directory>
</VirtualHost>


```

 mkdir -p /var/lib/pgadmin4/
 mkdir -p /var/log/pgadmin4/
 chown -R apache:apache /var/lib/pgadmin4
 chown -R apache:apache /var/log/pgadmin4



vim /usr/lib/python3.6/site-packages/pgadmin4-web/config_distro.py

LOG_FILE = '/var/log/pgadmin4/pgadmin4.log'
SQLITE_PATH = '/var/lib/pgadmin4/pgadmin4.db'
SESSION_DB_PATH = '/var/lib/pgadmin4/sessions'
STORAGE_DIR = '/var/lib/pgadmin4/storage'

python /usr/lib/python3.6/site-packages/pgadmin4-web/setup.py


NOTE: if getting error then

yum install python-psycopg2
yum -y install python-pip
yum -y install pgadmin4
