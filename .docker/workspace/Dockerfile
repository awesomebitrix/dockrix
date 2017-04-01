FROM php:7.0-fpm

MAINTAINER Igor Barkowsky <igor.barkowsky@gmail.com>

RUN apt-get update && apt-get install -y \
    software-properties-common \
    libpq-dev \
    libmemcached-dev \
    git \
    curl \
    vim \
    nano \
    wget \
    libjpeg-dev \
    libpng12-dev \
    libfreetype6-dev \
    libssl-dev \
    libmcrypt-dev \
    mysql-client \
    openssh-server \
    --no-install-recommends \
    && rm -r /var/lib/apt/lists/*

# install mcrypt library
RUN docker-php-ext-install mcrypt

# configure gd library
RUN docker-php-ext-configure gd \
    --enable-gd-native-ttf \
    --with-jpeg-dir=/usr/lib \
    --with-freetype-dir=/usr/include/freetype2

# Install extensions using the helper script provided by the base image
RUN docker-php-ext-install \
    opcache \
    mysqli \
    gd

# Install xdebug
RUN pecl install xdebug \
    && docker-php-ext-enable xdebug

# Add bin folder of composer to PATH.
RUN echo "export PATH=${PATH}:/var/www/site/vendor/bin" >> ~/.bashrc

# Install Composer
RUN curl -s http://getcomposer.org/installer | php \
    && mv composer.phar /usr/local/bin/composer

# Load xdebug Zend extension with phpunit command
RUN echo "alias phpunit='php -dzend_extension=xdebug.so /var/www/site/vendor/bin/phpunit'" >> ~/.bashrc

RUN usermod -u 1000 www-data

WORKDIR /var/www/site

RUN mkdir /var/run/sshd
RUN echo 'root:bitrix' | chpasswd
RUN sed -i 's/PermitRootLogin without-password/PermitRootLogin yes/' /etc/ssh/sshd_config

# SSH login fix. Otherwise user is kicked off after login
RUN sed 's@session\s*required\s*pam_loginuid.so@session optional pam_loginuid.so@g' -i /etc/pam.d/sshd

ENV NOTVISIBLE "in users profile"
RUN echo "export VISIBLE=now" >> /etc/profile

EXPOSE 22
CMD ["/usr/sbin/sshd", "-D"]