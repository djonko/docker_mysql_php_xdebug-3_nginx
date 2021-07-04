FROM php:7.4-fpm
ARG TIMEZONE
COPY php-fpm.d/www.conf /usr/local/etc/php-fpm.d/www.conf
RUN touch /tmp/xdebug.log \
        && chown www-data:www-data /tmp/xdebug.log \
        && chmod 666 /tmp/xdebug.log

RUN apt-get update && apt-get install -y \
    gnupg \
    inetutils-ping \
    telnet \
    telnetd \
    net-tools \
    g++ \
    procps \
    openssl \
    git \
    unzip \
    zlib1g-dev \
    libzip-dev \
    libfreetype6-dev \
    libpng-dev \
    libjpeg-dev \
    libicu-dev  \
    libonig-dev \
    libxslt1-dev \
    wget \
    acl

RUN pecl install xdebug \
    && docker-php-ext-enable xdebug && docker-php-ext-configure gd --with-jpeg --with-freetype && docker-php-ext-install \
    pdo_mysql zip xsl gd intl opcache exif

# Set timezone
RUN ln -snf /usr/share/zoneinfo/${TIMEZONE} /etc/localtime && echo ${TIMEZONE} > /etc/timezone \
    && printf '[PHP]\ndate.timezone = "%s"\n', ${TIMEZONE} > /usr/local/etc/php/conf.d/tzone.ini \
    && "date"


RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer
RUN  wget https://get.symfony.com/cli/installer -O - | bash
RUN mv /root/.symfony/bin/symfony /usr/local/bin/symfony



RUN symfony server:ca:install
EXPOSE 9000