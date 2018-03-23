由于通过supervisor来管理exporter的运行和自启动，所以本角色依赖supervisor角色，即meta/main.yml中指定了依赖关系，在安装exporter之前会自动先安装supervisor。


本playbook包含变量，除了myhosts和myuser之外，还有：

exporter_name 指定exporter名，本角色目前支持：consul_exporter,haproxy_exporter,memcached_exporter,node_exporter四个exporter

option_params 指定运行参数

option_params变量写法说明：

1) haproxy_exporter

option_params='--web.listen-address=:19101 --haproxy.scrape-uri unix:/var/lib/haproxy/stats'

说明：指定metrics端口为19101，同时需要指定haproxy应用的socket文件路径（此路径必须是在haproxy应用的配置文件有配置，这里要与其保持一致）

2) consul_exporter

option_params=''

说明：表示不指定特意参数，默认metrics端口为9107

3) memcached_exporter

option_params='--web.listen-address=:9150 --memcached.address=localhost:12000'

说明：指定metrics端口为9150，另外还需要指定memcached服务器的地址

4) node_exporter

option_params='--web.listen-address=:19100'

说明：指定metrics端口为19100

执行示例

1) haproxy_exporter

ansible-playbook main.yml -e "myhosts=192.168.10.105 myuser=admin exporter_name=haproxy_exporter option_params='--web.listen-address=:19101 --haproxy.scrape-uri unix:/var/lib/haproxy/stats'"

2) consul_exporter

ansible-playbook main.yml -e "myhosts=consul myuser=admin exporter_name=consul_exporter option_params=''"

3) memcached_exporter

ansible-playbook main.yml -e "myhosts=memcache myuser=admin exporter_name=memcached_exporter option_params='--web.listen-address=:9150 --memcached.address=localhost:12000'" -i /var/lvtu-deploy/ansible/hosts/memcache
