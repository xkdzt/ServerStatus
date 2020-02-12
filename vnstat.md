yum -y install gcc make
yum install sqlite sqlite-devel


git clone https://github.com/vergoh/vnstat.git
cd vnstat

./configure --prefix=/usr --sysconfdir=/etc && make


make install


