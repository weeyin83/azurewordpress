FROM ubuntu
MAINTAINER bartr

EXPOSE 80 443

WORKDIR /usr/local

RUN dpkg-divert --local --rename --add /sbin/initctl \
&&  ln -sf /bin/true /sbin/initctl \
&&  apt-get update \
&&  apt-get -y upgrade \
&&  apt-get -y install \
supervisor \
ca-certificates \
nginx \
libjpeg-dev \
libpng12-dev \
php7.0-fpm \
php7.0-mysql \
php7.0-curl \
#php7.0-gd \
#php7.0-intl \
#php7.0-mcrypt \
#php7.0-xmlrpc \
#php7.0-imap \
#php7.0-pspell \
#php7.0-recode \
#php7.0-tidy \
#php7.0-xsl \
#php-pear \
#php-imagick \
#php-memcache \
#php-sqlite3 \
#php-apcu \
nano \
curl \
git \
mysql-client-core-5.7

# nginx config
RUN sed -i -e "s/;cgi.fix_pathinfo=1/cgi.fix_pathinfo=0/g" /etc/php/7.0/fpm/php.ini \
&&  sed -i -e "s/upload_max_filesize\s*=\s*2M/upload_max_filesize = 100M/g" /etc/php/7.0/fpm/php.ini \
&&  sed -i -e "s/post_max_size\s*=\s*8M/post_max_size = 100M/g" /etc/php/7.0/fpm/php.ini \
&&  sed -i -e "s/;daemonize\s*=\s*yes/daemonize = no/g" /etc/php/7.0/fpm/php-fpm.conf \
&&  sed -i -e "s/;catch_workers_output\s*=\s*yes/catch_workers_output = yes/g" /etc/php/7.0/fpm/pool.d/www.conf \
&&  find /etc/php/7.0/cli/conf.d/ -name "*.ini" -exec sed -i -re 's/^(\s*)#(.*)/\1;\2/g' {} \; \
&&  sed -i -e "s@/run/php/php7.0-fpm.sock@127.0.0.1:9000@g" /etc/php/7.0/fpm/pool.d/www.conf \
&&  rm -Rf /etc/nginx/nginx.conf \
&&  mkdir -p /etc/nginx/sites-available/ \
&&  mkdir -p /etc/nginx/sites-enabled/ \
&&  rm /etc/nginx/sites-available/* \
&&  rm /etc/nginx/sites-enabled/* \
&& sed -i "s@;clear_env = no@clear_env = no@" /etc/php/7.0/fpm/pool.d/www.conf


# Let the conatiner know that there is no tty
#ENV DEBIAN_FRONTEND noninteractive

ENV GIT_REPO=https://github.com/bartr/wordpressfiles.git \
WORDPRESS_DB_HOST=westus1-a.control.database.windows.net \
WORDPRESS_DB_NAME=wordpress

COPY wprun.sh wp-config.php /usr/local/
COPY etc  /etc/

RUN chmod 755 /usr/local/wprun.sh \
&& ln -s /etc/nginx/sites-available/default.conf /etc/nginx/sites-enabled/default.conf \
&& echo "[client]\nssl-mode=VERIFY_CA\nssl-ca=/etc/ssl/certs/Baltimore_CyberTrust_Root.pem" > /etc/my.cnf

CMD ["/usr/local/wprun.sh"]

#RUN echo "host=mybartr.mysql.database.azure.com\nuser=bartr@mybartr\npassword=J0shMatt" >> /etc/my.cnf
ENV WORDPRESS_DB_USER=wordpress@mybartr WORDPRESS_DB_PASSWORD=2usxxWdbB2KRnWa4

#CMD ["bash"]
