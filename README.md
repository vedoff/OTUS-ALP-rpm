# Создание своего репозитория rpm пакетов

## Устанавливаем необходимые пакеты 

sudo yum install -y wget rpm-build rpmdevtools gcc make openssl-devel zlib-devel pcre-devel tree yum-utils createrepo redhat-lsb-core

## Создаем пользователя для сборки

sudo useradd builder -m

sudo su - builder

## Создаем дерево каталогов для сборки

rpmdev-setuptree

## Качаем нужные пакеты для сборки

wget https://nginx.org/packages/mainline/centos/7/SRPMS/nginx-1.19.3-1.el7.ngx.src.rpm

wget https://www.openssl.org/source/old/1.1.1/openssl-1.1.1a.tar.gz

tar -xf openssl-1.1.1a.tar.gz

## Получаем исходники nginx все исходники распакуются в требуемые каталоги каталоги подготовленные ранее.

rpm -Uvh nginx-1.19.3-1.el7.ngx.src.rpm

cd rpmbuild/SPECS/

## Правим nginx.spec
### vi rpmbuild/SPECS/nginx.spec

sed '123i<--with-openssl=/root/openssl-1.1.1a>' nginx.spec

### добавляем после %build
./configure
--with-openssl=/root/openssl-1.1.1a

# -------------------------------------

## Собираем пакет nginx

rpmbuild -bb nginx.spec

exit

sudo -i
## Каталог куда собирется пакет

cd /home/builder/rpmbuild/RPMS/x86_64

ls -l

## Устанавливаем пакет nginx

rpm -Uvh nginx-1.19.3-1.el7.ngx.x86_64.rpm

systemctl status nginx
systemctl start nginx
systemctl status nginx

## Проверяем под каким именем пакет встал в систему =============

rpm -qa | grep nginx


# ===== Создание репозитория =====
### ---действия производим под root

mkdir /usr/share/nginx/html/repo

cp /home/builder/rpmbuild/RPMS/x86_64/nginx-1.19.3-1.el7.ngx.x86_64.rpm /usr/share/nginx/html/repo/

ls -l

## Создадим репозиторий

createrepo /usr/share/nginx/html/repo/

nginx -s reload
## ===========================================

https://downloads.apache.org/httpd/httpd-2.4.51.tar.bz2

#=== Building RPMs 
rpmbuild -tb httpd-2.4.x.tar.bz2

#=== Creating a Source RPM ¶

rpmbuild -ts httpd-2.4.x.tar.bz2

## --------------------------------------------------------

## haproxy
https://www.haproxy.org/download/2.5/src/haproxy-2.5.0.tar.gz
