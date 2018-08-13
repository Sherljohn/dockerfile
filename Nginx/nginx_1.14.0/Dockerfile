# ******************************************
# ** 基于 centos 6.8 编译安装 Nginx 1.14.0 **
# ******************************************

FROM sherljohn/centos:6.8

MAINTAINER sherljohn@163.com

ENV NGINX_VERSION=1.14.0

# 创建一个没有 home 目录且不能登录的用户 www
RUN useradd -M -s /sbin/nologin www

# 复制并解压 nginx 源码
ADD nginx-$NGINX_VERSION.tar.gz /usr/local/src

WORKDIR /usr/local/src/nginx-$NGINX_VERSION
RUN yum -y install libxslt-devel libgeoip-dev \
	&& yum -y install gd gd-devel GeoIP GeoIP-devel pcre pcre-devel \
	&& yum -y install perl perl-devel perl-ExtUtils-Embed \
	&& yum -y install gcc gcc-c++ ncurses-devel \
	&& yum -y install openssl openssl-devel \
	&& ./configure --user=www --group=www --prefix=/usr/local/nginx \
				   --with-file-aio --with-compat --with-threads --with-mail --with-mail=dynamic --with-stream --with-stream=dynamic \
				   --with-select_module --with-poll_module --with-http_ssl_module --with-http_v2_module \
				   --with-http_realip_module --with-http_addition_module --with-http_xslt_module \
				   --with-http_xslt_module=dynamic --with-http_image_filter_module --with-http_sub_module \
				   --with-http_dav_module --with-http_flv_module --with-http_mp4_module --with-http_gunzip_module \
				   --with-http_gzip_static_module --with-http_auth_request_module --with-http_random_index_module \
				   --with-http_secure_link_module --with-http_degradation_module --with-http_slice_module \
				   --with-http_stub_status_module --with-http_perl_module --with-http_perl_module=dynamic \
				   --with-mail_ssl_module --with-stream_ssl_module --with-stream_realip_module --with-stream_ssl_preread_module \
    && make \
    && make install \
    && make clean \
	&& mkdir /usr/local/nginx/conf/vhosts \
	&& sed -i "116,117s/^}$/    include vhosts\/*.conf;\n}/" /usr/local/nginx/conf/nginx.conf \
	&& cd /usr/local/src/ \
	&& rm -fr * \
	&& yum clean all \
	&& echo -e "\n" >> /etc/profile \
	&& echo 'export PATH="$PATH:/usr/local/nginx/sbin"' >> /etc/profile \
	&& source /etc/profile

RUN yum install -y iputils \
	&& yum install -y telnet \
	&& yum clean all

EXPOSE 80

CMD ["/usr/local/nginx/sbin/nginx", "-g", "daemon off;"]
