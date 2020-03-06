Piwik Docker Container
# 設定Docker Link讓web可以連結回各自主機的Docker DB

docker run --name piwik1 -v /data/piwik1:/var/www/html --link mysql-s1:db -p 1234:80  --restart always -d piwik

docker run --name piwik2 -v /data/piwik2:/var/www/html --link mysql-s2:db -p 2234:80  --restart always -d piwik


MySQL Docker Container
docker run --name mysql-s1 -p 4306:3306 -e MYSQL_ROOT_PASSWORD=1q2w3e4r5t_ -v /data/mysql1_data:/var/lib/mysql -v /data/server1/conf.d:/etc/mysql/mysql.conf.d/ --restart always -d mysql

docker run --name mysql-s2 -p 4307:3306 -e MYSQL_ROOT_PASSWORD=1q2w3e4r5t_ -v /data/mysql2_data:/var/lib/mysql -v /data/server2/conf.d:/etc/mysql/mysql.conf.d/ --restart always -d mysql

mysql > GRANT REPLICATION SLAVE ON *.* TO 'replica'@'%' IDENTIFIED BY '1q2w3e4r5t_';
mysql > GRANT REPLICATION SLAVE ON *.* TO 'replica'@'%' IDENTIFIED BY '1q2w3e4r5t_';
mysql > 
mysql > FLUSH PRIVILEGES;
mysql > show master status;
mysql > 
mysql > CHANGE MASTER TO MASTER_HOST='srvealablinux-d', MASTER_PORT=3306,MASTER_USER='replica', MASTER_PASSWORD='1q2w3e4r5t_', MASTER_LOG_FILE='XXX', MASTER_LOG_POS=XXX;
mysql > 
mysql > CHANGE MASTER TO MASTER_HOST='srvealablinux-d', MASTER_PORT=3307,MASTER_USER='replica', MASTER_PASSWORD='1q2w3e4r5t_', MASTER_LOG_FILE='XXX', MASTER_LOG_POS=XXX;

# XXX 需替換為mysql回應 MASTER_LOG Information
#
# 啟動 slave 
#
mysql> START slave;
#
# 觀看 slave 狀態 
#
mysql> show slave status \G;

#必須為YES

Slave_IO_Running: Yes

Slave_SQL_Running: Yes
