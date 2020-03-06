Create a PMM Data Container
docker create \
	-v /opt/prometheus/data \
	-v /opt/consul-data \
	-v /var/lib/mysql \
	-v /var/lib/grafana \
	--name pmm-data \
	percona/pmm-server:latest /bin/true

Create and Run the PMM Server Container
docker run -d \
	-p 8888:80 \
	--volumes-from pmm-data \
	--name pmm-server \
	--restart always \
	percona/pmm-server:latest

# rpm -ivh [Package Name] CentOS
# dpkg -i [Package Name]  Deb

pmm-admin config --server [server ip]:[server port] --bind-address [container private port] --client-address [container public ip]

# MySQL InnoDB Metrics
# mysql > SET GLOBAL innodb_monitor_enable=all;
# After install remember restart docker
# pmm-admin add --help

