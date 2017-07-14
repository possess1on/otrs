# otrs
install and tune OTRS 5 on Ubuntu Server

apt-get update
apt-get upgrade
apt-get install openssh-server
wget ftp://ftp.otrs.org/pub/otrs/otrs-5.0.6.tar.gz
tar -xvzf otrs-5.0.6.tar.gz
mv otrs-5.0.6 /opt/otrs
apt-get install apache2 fetchmail libapache2-mod-perl2 libdbd-mysql-perl libnet-dns-perl libnet-ldap-perl libio-socket-ssl-perl libpdf-api2-perl libsoap-lite-perl libgd-text-perl libgd-graph-perl libapache-dbi-perl libyaml-libyaml-perl libtemplate-perl libarchive-zip-perl liblwp-useragent-determined-perl libapache2-reload-perl libnet-smtp-ssl-perl libnet-smtp-tls-butmaintained-perl libgd-gd2-perl libjson-xs-perl libpdf-api2-simple-perl libtext-csv-xs-perl libxml-parser-perl libmail-imapclient-perl mysql-server
useradd -d /opt/otrs/ -c 'OTRS user' otrs
usermod -G www-data otrs
cp /opt/otrs/Kernel/Config.pm.dist /opt/otrs/Kernel/Config.pm
/opt/otrs/bin/otrs.SetPermissions.pl --web-group=www-data
/opt/otrs/bin/otrs.CheckModules.pl
perl -cw /opt/otrs/bin/cgi-bin/index.pl
perl -cw /opt/otrs/bin/cgi-bin/customer.pl
perl -cw /opt/otrs/bin/otrs.PostMaster.pl
ln -s /opt/otrs/scripts/apache2-httpd.include.conf /etc/apache2/sites-available/otrs.conf
a2ensite otrs.conf
service apache2 reload
nano /etc/mysql/my.cnf
---------------------------------
[mysqld] 
max_allowed_packet = 20M
query_cache_size = 32M
innodb_log_file_size = 256M
----------------------------------
service mysql stop
rm /var/lib/mysql/ib_logfile0
rm /var/lib/mysql/ib_logfile1
service mysql start
---------------------------------
http://0.0.0.0/otrs/installer.pl
В пункте “SystemID” выбираем “10”.
В пункте “CheckMXRecord” выбираем “No”.
---------------------------------
/opt/otrs/var/cron” командой «cd /opt/otrs/var/cron
for foo in *.dist; do cp $foo `basename $foo .dist`; done
su otrs
/opt/otrs/bin/otrs.Daemon.pl start
/opt/otrs/bin/Cron.sh start
crontab -l
