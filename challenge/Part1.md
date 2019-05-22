sudo useradd training -u 3800
sudo passwd training    

grep training /etc/passwd
grep training /etc/group

tail /etc/group
sudo groupadd skcc
sudo usermod -a -G skcc training
sudo usermod -aG wheel training




sudo vi /ets/hosts
172.31.12.1     ts1.test.com    ts1
172.31.10.5     ts2.test.com    ts2
172.31.5.97     ts3.test.com    ts3
172.31.11.59    ts4.test.com    ts4
172.31.14.239   ts5.test.com    ts5

uname -mrs

df 
du

yum repolist enabled

sudo cat /etc/passwd



[install mySQL server]

sudo yum install mariadb-server
sudo systemctl stop mariadb
sudo systemctl enable mariadb
sudo systemctl start mariadb
sudo /usr/bin/mysql_secure_installation


[install jdk]


yum list java*jdk-devel
sudo yum install java-1.8.0-openjdk-devel.x86_64
rpm -qa java*jdk-devel
javac -version


[install CM]


sudo yum install wget
sudo wget https://archive.cloudera.com/cm5/redhat/7/x86_64/cm/5.16.1/ -P /etc/yum.repos.d/
baseurl=https://archive.cloudera.com/cm5/redhat/7/x86_64/cm/5.15.2/
sudo rpm --import \
https://archive.cloudera.com/cm5/redhat/7/x86_64/cm/RPM-GPG-KEY-cloudera
sudo yum install cloudera-manager-daemons cloudera-manager-server


[install the mysql connector]

sudo wget "https://dev.mysql.com/get/Downloads/Connector-J/mysql-connector-java-5.1.46.tar.gz"
tar zxvf mysql-connector-java-5.1.46.tar.gz
sudo mkdir -p /usr/share/java/
cd mysql-connector-java-5.1.46
sudo cp mysql-connector-java-5.1.46-bin.jar /usr/share/java/mysql-connector-java.jar


[setup database]

mysql -u root -p adimin
drop database if exists amon;
create database amon character set utf8;
grant all privileges on amon.* to amon@'%' identified by 'amon_password';

drop database if exists rman;
create database rman character set utf8;
grant all privileges on rman.* to rman@'%' identified by 'rman_password';

drop database if exists metastore;
create database metastore character set utf8;
grant all privileges on metastore.* to hive@'%' identified by 'hive_password';

drop database if exists sentry;
create database sentry character set utf8;
grant all privileges on sentry.* to sentry@'%' identified by 'sentry_password';

drop database if exists nav;
create database nav character set utf8;
grant all privileges on nav.* to nav@'%' identified by 'nav_password';

drop database if exists navms;
create database navms character set utf8;
grant all privileges on navms.* to navms@'%' identified by 'navms_password';

drop database if exists cdh;
create database cdh character set utf8;
grant all privileges on cdh.* to cdh@'%' identified by 'cdh';

drop database if exists hue;
create database hue character set utf8;
grant all privileges on hue.* to hue@'%' identified by 'hue';

drop database if exists oozie;
create database oozie character set utf8;
grant all privileges on oozie.* to oozie@'%' identified by 'oozie';

drop database if exists hive;
create database hive character set utf8;
grant all privileges on hive.* to hive@'%' identified by 'hive';

drop database if exists scm;
create database scm character set utf8;
grant all privileges on scm.* to scm@'%' identified by 'scm';

exit;

sudo /usr/share/cmf/schema/scm_prepare_database.sh mysql scm scm

sudo systemctl stop cloudera-scm-server
sudo systemctl start cloudera-scm-server
sudo tail -f /var/log/cloudera-scm-server/cloudera-scm-server.log

sudo yum install -y cloudera-manager-daemons cloudera-manager-agent
sudo systemctl start cloudera-scm-agent


[create table in MySQL]

create database test;
use test;
(created table using authors.sql and posts.sql)
create user 'training'@'%' identified by 'training';
grant all privileges on *.* to 'training'@'%';



