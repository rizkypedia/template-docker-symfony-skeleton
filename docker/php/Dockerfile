ARG PHP_VERSION=8.1.18

FROM php:${PHP_VERSION}-fpm

ARG PHP_MEMORY_LIMIT=2G

RUN apt-get update \
    && apt-get install -y --no-install-recommends vim curl debconf subversion git apt-transport-https apt-utils \
    build-essential locales acl mailutils wget nodejs zlib1g-dev zip unzip \
    gnupg gnupg1 gnupg2 libzip-dev libicu-dev npm curl

RUN docker-php-ext-install pdo pdo_mysql zip intl opcache \
    && pecl install xdebug \
    && pecl install apcu \
    && pecl install pcov \
    && docker-php-ext-enable xdebug apcu opcache\
    && echo "xdebug.idekey=PHPSTORM" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini && \
    echo "xdebug.profiler_enable=on" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini  && \
    echo "xdebug.remote_enable=on" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini && \
    echo "xdebug.remote_handler=dbgp" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini && \
    echo "xdebug.remote_mode=req" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini && \
    echo "xdebug.remote_port=9005" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini && \
    echo "xdebug.remote_host=host.docker.internal" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini && \
    echo "xdebug.remote_autostart=1" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini && \
    echo "xdebug.remote_connect_back=1" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini && \
    echo "xdebug.client_host = host.docker.internal" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini && \
    echo "xdebug.mode=debug" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini

COPY php.ini /etc/php/${PHP_VERSION}/php.ini
COPY php.ini /usr/local/etc/php/php.ini
COPY php-fpm-pool.conf /etc/php/${PHP_VERSION}/pool.d/www.conf
COPY opcache.ini /usr/local/etc/php/conf.d/opcache.ini

RUN curl -sSk https://getcomposer.org/installer | php -- --disable-tls && \
	mv composer.phar /usr/local/bin/composer

RUN curl -sS https://get.symfony.com/cli/installer | bash
RUN mv /root/.symfony5/bin/symfony /usr/local/bin/symfony

RUN wget --no-check-certificate https://phar.phpunit.de/phpunit-6.5.3.phar && \
    mv phpunit*.phar phpunit.phar && \
    chmod +x phpunit.phar && \
    mv phpunit.phar /usr/local/bin/phpunit

RUN wget --no-check-certificate https://github.com/fabpot/local-php-security-checker/releases/download/v1.0.0/local-php-security-checker_1.0.0_linux_386 && \
    mv local-php-security-checker_1.0.0_linux_386 local-php-security-checker && \
    chmod +x local-php-security-checker && \
    mv local-php-security-checker /usr/local/bin/local-php-security-checker

RUN curl -sL https://deb.nodesource.com/setup_18.x  | bash -
RUN apt-get -y install nodejs

RUN npm install yarn

RUN groupadd dev -g 999
RUN useradd dev -g dev -d /home/dev -m

RUN rm -rf /var/lib/apt/lists/*
RUN echo "en_US.UTF-8 UTF-8" > /etc/locale.gen && \
    echo "fr_FR.UTF-8 UTF-8" >> /etc/locale.gen && \
    locale-gen


WORKDIR /home/wwwroot/project

EXPOSE 9000
CMD ["php-fpm"]
