# ******************************************
# **  基于 centos 6.8 编译安装 PHP 5.6.37  **
# ******************************************

FROM sherljohn/centos:6.8

MAINTAINER sherljohn@163.com

ENV PHP_VERSION=5.6.37

RUN yum update -y -y \
    && yum install -y wget gcc gcc-c++ make openssl-devel \
    && yum install -y bison bison-devel zlib-devel libmcrypt-devel mcrypt mhash-devel libxml2-devel libcurl-devel bzip2-devel readline-devel libedit-devel sqlite-devel \
	&& yum install -y libjpeg-devel \
    && yum install -y libpng-devel \
    && yum install -y freetype-devel \
    && yum install -y libc-client-devel \
    && yum install -y openldap-devel \
    && yum install -y postgresql-devel \
    && yum install -y libxslt-devel* \
    && yum install -y libmemcached \
    && yum install -y telnet \
	&& cd /usr/local/src/ \
	&& wget http://cn2.php.net/distributions/php-$PHP_VERSION.tar.gz \
	&& wget ftp://mcrypt.hellug.gr/pub/crypto/mcrypt/attic/libmcrypt/libmcrypt-2.5.7.tar.gz \
	&& tar xf php-$PHP_VERSION.tar.gz \
	&& tar xf libmcrypt-2.5.7.tar.gz \
    && cd libmcrypt-2.5.7 \
    && ./configure --prefix=/usr/local \
    && make \
    && make install \
	&& make clean \
    && echo "/usr/local/lib" >> /etc/ld.so.conf.d/local.conf \
    && ldconfig -v \
	&& cd ../php-$PHP_VERSION/ \
    && ./configure --prefix=/usr/local/php --with-config-file-path=/usr/local/php/etc --with-zlib-dir --with-freetype-dir --enable-mbstring --with-libxml-dir=/usr/local/libxml \
                   --enable-soap --enable-calendar --with-curl --with-mcrypt --with-zlib --with-gd --disable-rpath --enable-inline-optimization \
                   --with-bz2 --with-zlib --enable-sockets --enable-sysvsem --enable-sysvshm --enable-pcntl --with-ldap --enable-mbregex \
                   --enable-exif --enable-bcmath --with-mhash --enable-zip --with-pcre-regex --with-mysql --with-pdo-mysql --with-mysqli \
                   --with-pdo-pgsql --with-pdo-sqlite --enable-pdo --with-jpeg-dir=/usr/local/libjpeg --with-png-dir=/usr/local/libpng \
                   --enable-gd-native-ttf --with-openssl --with-fpm-user=www --with-fpm-group=www --with-libdir=lib64 --enable-ftp --with-imap \
                   --with-imap-ssl --with-kerberos --with-gettext --with-xmlrpc --with-xsl --enable-opcache --enable-fpm --enable-xml \
                   --enable-shmop --enable-session --enable-ctype --with-iconv-dir --with-iconv \
	&& make \
	&& make install \
	&& make clean \
	&& cp php.ini-production /usr/local/php/etc/php.ini \
	&& cp php.ini-production /usr/local/php/etc/php.ini-production \
	&& cp php.ini-development /usr/local/php/etc/php.ini-development \
	&& cd /usr/local/php/etc \
	&& cp php-fpm.conf.default php-fpm.conf \
	&& sed -i 's/;daemonize = yes/daemonize = no/' /usr/local/php/etc/php-fpm.conf \
	&& sed -i 's/listen = 127.0.0.1:9000/listen = 0.0.0.0:9000/' /usr/local/php/etc/php-fpm.conf \
	&& cd /usr/local/src/ \
	&& wget https://pecl.php.net/get/memcache-2.2.7.tgz \
	&& tar xf memcache-2.2.7.tgz \
	&& cd memcache-2.2.7 \
	&& /usr/local/php/bin/phpize \
	&& ./configure --enable-memcache --with-php-config=/usr/local/php/bin/php-config --with-zlib-dir \
	&& make \
	&& make install \
	&& make clean \
	&& cd /usr/local/src \
    && wget https://launchpad.net/libmemcached/1.0/1.0.18/+download/libmemcached-1.0.18.tar.gz \
    && tar -zxvf libmemcached-1.0.18.tar.gz \
    && cd libmemcached-1.0.18\
    && ./configure --prefix=/usr/local/libmemcached \
    && make \
    && make install \
    && make clean \
    && cd /usr/local/src/ \
    && wget https://pecl.php.net/get/memcached-2.2.0.tgz \
    && tar xf memcached-2.2.0.tgz \
    && cd memcached-2.2.0 \
    && /usr/local/php/bin/phpize \
    && ./configure --enable-memcached --with-libmemcached-dir=/usr/local/libmemcached --with-php-config=/usr/local/php/bin/php-config \
    && make \
    && make install \
    && make clean \
	&& cd /usr/local/src/ \
    && wget https://pecl.php.net/get/xdebug-2.5.5.tgz \
    && tar xf xdebug-2.5.5.tgz \
    && cd xdebug-2.5.5 \
    && /usr/local/php/bin/phpize \
    && ./configure --enable-xdebug --with-php-config=/usr/local/php/bin/php-config \
    && make \
    && make install \
    && make clean \
    && cd /usr/local/src/ \
    && wget https://pecl.php.net/get/igbinary-2.0.7.tgz \
    && tar xf igbinary-2.0.7.tgz \
    && cd igbinary-2.0.7 \
    && /usr/local/php/bin/phpize \
    && ./configure --enable-igbinary --with-php-config=/usr/local/php/bin/php-config \
    && make \
    && make install \
    && make clean \
    && cd /usr/local/src/ \
    && wget https://pecl.php.net/get/redis-3.1.4.tgz \
    && tar xf redis-3.1.4.tgz \
    && cd redis-3.1.4 \
    && /usr/local/php/bin/phpize \
    && ./configure --enable-redis --with-php-config=/usr/local/php/bin/php-config \
    && make \
    && make install \
    && make clean \
	&& groupadd www \
    && useradd -g www www \
    && cp /usr/local/src/php-$PHP_VERSION/sapi/fpm/init.d.php-fpm /etc/init.d/php-fpm \
    && chmod 755 /etc/init.d/php-fpm \
	&& cd /usr/local/src/ \
	&& rm -fr * \
	&& yum clean all \
	&& echo -e "\n" >> /etc/profile \
	&& echo 'export PATH="$PATH:/usr/local/php/bin:/usr/local/php/sbin"' >> /etc/profile \
	&& source /etc/profile

EXPOSE 9000

ENTRYPOINT ["/usr/local/php/sbin/php-fpm"]