# Assignment08
I have create one droplet name mysql-assignment which is located Singapore.
 That droplet information:
 Droplet Name: mysql-Assignment IP Address: 128.199.222.30
Username: root
Password: 0497367af8bb580d101e10bd6a
Location: Singapur


I have create another droplet name mysql-amsterdam which is located Amsterdm

The droplet information:
Droplet Name: mysql-Amsterdam, IP Address: 178.62.217.195
Username: root
Password: b00f052f43af8da02ea486a8e2


Master slave information:

Droplet Name: mysql-Assignment IP Address: 128.199.222.30   ----master Database
Droplet Name: mysql-Amsterdam, IP Address: 178.62.217.195  -----Slave Database

I have install putty to connect remotely:

 




 I have use mysql-Assignment IP Address: 128.199.222.30 connect  from my local mysql workbench to  mysql-Assignment –Singapore—droplet.
 

Run classicmodels database script from my mysql workbench.

 

Configure the Master Database:
Open up mysql configuration file on the master server
sudo nano /etc/mysql/mysql.conf.d/mysqld.cnf
find the section bind-address and replace :
bind-address            = 128.199.222.30
Uncommented the following line:
server-id               = 1
log_bin                 = /var/log/mysql/mysql-bin.log
restart mysqlserver:
sudo service mysql restart

you can see now master status

 

We need to grant privileges to the slave. To do this 

GRANT REPLICATION SLAVE ON *.* TO 'slave_user'@'%' IDENTIFIED BY 'password';


Configure the Slave Database:
First step is create classicmodels database. 

Open up mysql configuration file on the slave server
sudo nano /etc/mysql/mysql.conf.d/mysqld.cnf

find the section bind-address and replace :
bind-address            = 178.62.217.195
Uncommented the following line:
server-id               = 2
log_bin                 = /var/log/mysql/mysql-bin.log
binlog_do_db            = classicmodels

restart mysqlserver:
sudo service mysql restart


The next step is to enable the replication from mysql shall
Open up the ysql shall and type following details and run


CHANGE MASTER TO
 MASTER_HOST='128.199.222.30',
MASTER_USER='slave_user
', MASTER_PASSWORD='password', MASTER_LOG_FILE='mysql-bin.000001', MASTER_LOG_POS=  107;


Activate slave server type the following command

Start slave;

If you want to see details of the slave replication by typing the following command

SHOW SLAVE STATUS\G


Now if you will insert, delete or update from master then slave server also change.








