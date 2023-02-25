FROM php:8.2-apache-bullseye

RUN apt update -y && apt upgrade -y;
RUN apt install -y libxml2-dev libpng-dev curl libcurl4-gnutls-dev libxslt-dev libzip-dev libtidy-dev
RUN a2enmod ssl actions headers alias rewrite expires
RUN a2ensite default-ssl
RUN docker-php-ext-install -j$(nproc) xml xmlrpc opcache bcmath mbstring gd curl xsl intl zip tidy mysqli pdo pdo_mysql

RUN pecl install xdebug-3.2.0 \
    && docker-php-ext-enable xdebug

# dev.sandbox.com
RUN ln -s "/opt/projects/dev.sandbox.com" "/var/www/dev.sandbox.com"
RUN openssl genrsa -out /etc/ssl/certs/dev.sandbox.com-selfsigned.key 2048
RUN openssl req -new -x509 -key /etc/ssl/certs/dev.sandbox.com-selfsigned.key -out /etc/ssl/certs/dev.sandbox.com-selfsigned.crt -days 3650 -subj /CN=dev.sandbox.com

# Enable .htaccess
RUN sed -i 's/AllowOverride None/AllowOverride All/g' /etc/apache2/apache2.conf
RUN sed -i 's/150/500/g' /etc/apache2/mods-enabled/mpm_prefork.conf

#CMD ["apache2ctl", "-k", "graceful"]
#RUN kill -USR1 1