# Tengine
groupadd html
useradd -g html -s /sbin/nologin -d /var/www wangyan

libxml2/libxslt
yum groupinstall "Development Tools"
yum install pcre-devel openssl-devel libxml2-devel libxslt-devel gd-devel lua-devel geoip-devel
pcre-dev

./configure \
--prefix=/usr/local/nginx \
--user=wangyan \
--group=html \
--enable-mods-shared=all