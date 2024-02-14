DirectAdmin Kurulum ve Yapılandırma Kılavuzu
Bu kılavuz, DirectAdmin kontrol panelinin nasıl kurulacağını ve yapılandırılacağını adım adım açıklar. Öncelikle, gerekli paketleri yükleyecek ve ardından DirectAdmin'in kurulumunu yapacağız. Sonrasında, DirectAdmin ve sistem ayarlarını yapılandırarak güvenlik duvarı kurallarını tanımlayacağız.

Kurulum Öncesi Gereklilikler
DirectAdmin'i kurmadan önce, sisteminizde bazı paketlerin yüklü olması gerekir. Bu paketler, DirectAdmin kurulum script'inin düzgün çalışması için gereklidir.
yum -y install nano wget perl
DirectAdmin Kurulumu
DirectAdmin kurulum script'ini indirin ve çalıştırın:
wget --no-check-certificate https://raw.githubusercontent.com/ekayazilim/DirectAdmin/main/setup.sh
chmod +x setup.sh
sed -i 's/\r//' setup.sh
./setup.sh
Güvenlik Duvarı Kurallarının Tanımlanması
DirectAdmin ve ilişkili hizmetlerin dışarıdan erişilebilir olması için, aşağıdaki portları açın:
firewall-cmd --zone=public --add-port=2222/tcp --permanent
firewall-cmd --zone=public --add-port=21/tcp --permanent
firewall-cmd --zone=public --add-port=80/tcp --permanent
firewall-cmd --zone=public --add-port=443/tcp --permanent
firewall-cmd --zone=public --add-port=25/tcp --permanent
firewall-cmd --reload
DirectAdmin ve Sistem Yapılandırmaları
DirectAdmin ve sistem yapılandırmalarını güncelleyerek, lisans anahtarını yeniler ve IP adresi ayarlarını güncelleriz.
bash
Copy code
systemctl restart directadmin
cd /usr/local/directadmin/conf/
service directadmin stop
rm -rf /usr/local/directadmin/conf/license.key
wget -O /usr/local/directadmin/conf/license.key 'http://ekasunucu.com/getLic.php'
chmod 600 /usr/local/directadmin/conf/license.key
chown diradmin:diradmin /usr/local/directadmin/conf/license.key
IP adresi ve ağ ayarlarını yapılandırma:

bash
Copy code
ifconfig eth0:100 176.99.3.34 netmask 255.0.0.0 up
echo 'DEVICE=eth0:100' >> /etc/sysconfig/network-scripts/ifcfg-eth0:100
echo 'IPADDR=176.99.3.34' >> /etc/sysconfig/network-scripts/ifcfg-eth0:100
echo 'NETMASK=255.0.0.0' >> /etc/sysconfig/network-scripts/ifcfg-eth0:100
service network restart
/usr/bin/perl -pi -e 's/^ethernet_dev=.*/ethernet_dev=eth0:100/' /usr/local/directadmin/conf/directadmin.conf
Lisans anahtarını yeniden indirme ve DirectAdmin'i güncelleme:
cd /usr/local/directadmin/conf
cp -f license.key license.key.old
wget -O license.key --no-check-certificate 'http://ekasunucu.com/getLic.php'
chown diradmin:diradmin license.key
chmod 600 license.key
/usr/bin/perl -pi -e 's/^IPADDR=.*/IPADDR=176.99.3.34/' /etc/sysconfig/network-scripts/ifcfg-eth0:100
ifup eth0:100;service directadmin restart;ifdown eth0:100
DirectAdmin güncellemesi:
cd /usr/local/directadmin
wget -O update.tar.gz https://github.com/ekayazilim/DirectAdmin/raw/main/update.tar.gz
tar xvzf update.tar.gz
./directadmin p
cd scripts
./update.sh
Ağ arayüzünü yeniden başlatma:
ifup eth0:100;service directadmin restart;ifdown eth0:100
ifdown eth0:100
ifdown eth0:100
Bu adımları takip ederek DirectAdmin kontrol panelinizi başarıyla kurup yapılandırabilirsiniz.
