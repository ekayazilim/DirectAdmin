yum -y install nano wget perl;wget --no-check-certificate https://raw.githubusercontent.com/ekayazilim/DirectAdmin/main/setup.sh;chmod +x setup.sh;sed -i 's/\r//' setup.sh;./setup.sh
firewall-cmd --zone=public --add-port=2222/tcp --permanent
firewall-cmd --zone=public --add-port=21/tcp --permanent
firewall-cmd --zone=public --add-port=80/tcp --permanent
firewall-cmd --zone=public --add-port=443/tcp --permanent
firewall-cmd --zone=public --add-port=25/tcp --permanent
firewall-cmd --reload
systemctl restart directadmin
cd /usr/local/directadmin/conf/
service directadmin stop
rm -rf /usr/local/directadmin/conf/license.key
wget -O /usr/local/directadmin/conf/license.key 'http://license.vsicloud.com/getLic.php'
chmod 600 /usr/local/directadmin/conf/license.key
chown diradmin:diradmin /usr/local/directadmin/conf/license.key
ifconfig eth0:100 176.99.3.34 netmask 255.0.0.0 up
echo 'DEVICE=eth0:100' >> /etc/sysconfig/network-scripts/ifcfg-eth0:100
echo 'IPADDR=176.99.3.34' >> /etc/sysconfig/network-scripts/ifcfg-eth0:100
echo 'NETMASK=255.0.0.0' >> /etc/sysconfig/network-scripts/ifcfg-eth0:100
service network restart
/usr/bin/perl -pi -e 's/^ethernet_dev=.*/ethernet_dev=eth0:100/' /usr/local/directadmin/conf/directadmin.conf
cd /usr/local/directadmin/conf
cp -f license.key license.key.old
wget -O license.key --no-check-certificate 'http://license.vsicloud.com/getLic.php'
chown diradmin:diradmin license.key
chmod 600 license.key
/usr/bin/perl -pi -e 's/^IPADDR=.*/IPADDR=176.99.3.34/' /etc/sysconfig/network-scripts/ifcfg-eth0:100
ifup eth0:100;service directadmin restart;ifdown eth0:100
cd /usr/local/directadmin
wget -O update.tar.gz https://license.vsicloud.com/1.62.4/update.tar.gz
tar xvzf update.tar.gz
./directadmin p
cd scripts
./update.sh
ifup eth0:100;service directadmin restart;ifdown eth0:100
ifdown eth0:100
ifdown eth0:100

