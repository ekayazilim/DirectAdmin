# DirectAdmin Kurulum ve Yapılandırma Kılavuzu

Bu kılavuz, DirectAdmin kontrol panelinin nasıl kurulacağını ve yapılandırılacağını adım adım açıklar. Öncelikle, gerekli paketleri yükleyecek ve ardından DirectAdmin'in kurulumunu yapacağız. Sonrasında, DirectAdmin ve sistem ayarlarını yapılandırarak güvenlik duvarı kurallarını tanımlayacağız.

## Kurulum Öncesi Gereklilikler

DirectAdmin'i kurmadan önce, sisteminizde bazı paketlerin yüklü olması gerekir. Bu paketler, DirectAdmin kurulum script'inin düzgün çalışması için gereklidir.

```bash
yum -y install nano wget perl

# DirectAdmin Kurulumu
DirectAdmin kurulum script'ini indirin ve çalıştırın:

```bash
wget --no-check-certificate https://raw.githubusercontent.com/ekayazilim/DirectAdmin/main/setup.sh
chmod +x setup.sh
sed -i 's/\r//' setup.sh
./setup.sh
