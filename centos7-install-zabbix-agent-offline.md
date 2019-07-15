# CentOS7 离线安装Zabbix Agent

步骤：
1. 下载zabbix agent离线安装包
2. 将包拷贝到离线机器
3. 安装zabbix agent


## 1. 下载zabbix agent离线安装包

```shell
mkdir zabbix
yum install --downloadonly --downloaddir=zabbix/ zabbix-agent
```

# 2. 将包拷贝到离线机器
scp上去即可

# 3. 安装zabbix agent
rpm -Uvh zabbix-agent-4.2.4-1.el7.x86_64.rpm
vi /etc/zabbix/zabbix_agentd.conf
# 修改Server、ServerActive、Hostname配置项
systemctl enable zabbix-agent
systemctl start zabbix-agent


Ansible脚本：
server_ip=10.10.10.2
agent_ip=10.10.10.10
ansible all -m shell -a 'rpm -Uvh https://repo.zabbix.com/zabbix/4.2/rhel/7/x86_64/zabbix-release-4.2-1.el7.noarch.rpm'
ansible all -m shell -a 'yum install -y zabbix-agent zabbix-sender'
ansible all -m shell -a "sed -i 's/Server=127.0.0.1/Server=${server_ip}/' /etc/zabbix/zabbix_agentd.conf "
ansible all -m shell -a "sed -i 's/ServerActive=127.0.0.1/ServerActive=${server_ip}/' /etc/zabbix/zabbix_agentd.conf "
ansible local -m shell -a "sed -i 's/Hostname=.*/Hostname=${agent_ip}/';"
ansible local -m shell -a "systemctl enable zabbix-agent"
ansible local -m shell -a "systemctl start zabbix-agent"