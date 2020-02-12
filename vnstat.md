yum -y install gcc make
yum install sqlite sqlite-devel


git clone https://github.com/vergoh/vnstat.git
cd vnstat

./configure --prefix=/usr --sysconfdir=/etc && make


make install

cp -v examples/init.d/redhat/vnstat /etc/init.d/
chkconfig vnstat on
service vnstat start

如遇到 Failed to restart vnstat.service: Unit vnstat.service is masked. 请删除 /etc/systemd/system/ 下的 vnstat.service 文件。

源安装

yum install epel-release -y
yum install -y vnstat


输入 ifconfig 命令查看自己的网卡名。一般来说 OVZ 的网卡是 venet0，而 XEN 和 KVM 的网卡是 eth0。


vi /etc/vnstat.conf
修改 Interface 选项

## KVM / XEN
Interface "eth0"

## OpenVZ
Interface "venet0"

MonthRotate 为每月流量结算日期，也就是每月流量重新计算的日期，默认为每月 1 日，根据需要修改。

修改好配置后使用 service vnstat restart 命令来重启 vn­Stat。


生成数据库
## KVM / XEN
vnstat -u -i eth0

## OpenVZ
vnstat -u -i venet0

数据库目录：/var/lib/vnstat/

删除数据库 vnstat --delete --force -i eth0

流量统计查询
vnstat -h    #按小时查询
vnstat -d    #按天数查询
vnstat -m    #按月数查询
vnstat -w    #按周数查询
vnstat -t    #查询TOP10

查询实时流量
## KVM / XEN
vnstat -l -i eth0 -ru

## OpenVZ
vnstat -l -i venet0 -ru

服务命令
启动 vnStat：service vnstat start

停止 vnStat：service vnstat stop

重启 vnStat：service vnstat restart

查看 vnStat 状态：service vnstat status
