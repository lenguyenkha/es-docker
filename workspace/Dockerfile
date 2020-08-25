FROM php:7.4-fpm


# install all the dependencies and enable PHP modules
RUN apt-get update && apt-get upgrade -y && apt-get install -y \
      procps \
      nano \
      git \
      unzip \
      libicu-dev \
      zlib1g-dev \
      libxml2 \
      libxml2-dev \
      libreadline-dev \
      supervisor \
      cron \
      libzip-dev \
    && pecl install xdebug \
    && docker-php-ext-enable xdebug \
    && docker-php-ext-configure pdo_mysql --with-pdo-mysql=mysqlnd \
    && docker-php-ext-configure intl \
    && docker-php-ext-install \
      pdo_mysql \
      sockets \
      intl \
      opcache \
      zip \
    && rm -rf /tmp/* \
    && rm -rf /var/list/apt/* \
    && rm -rf /var/lib/apt/lists/* \
    && apt-get clean

# Copy xdebug configuration for remote debugging
COPY config/xdebug.ini /usr/local/etc/php/conf.d/xdebug.ini

RUN sed -i "s/xdebug.remote_autostart=0/xdebug.remote_autostart=1/" /usr/local/etc/php/conf.d/xdebug.ini && \
    sed -i "s/xdebug.remote_enable=0/xdebug.remote_enable=1/" /usr/local/etc/php/conf.d/xdebug.ini && \
    sed -i "s/xdebug.cli_color=0/xdebug.cli_color=1/" /usr/local/etc/php/conf.d/xdebug.ini

# Composer, PHP loc, PHPmd, PHPCS, PHPCPD, PHPCBF
RUN curl -sS https://getcomposer.org/installer | php && mv composer.phar /usr/local/bin/composer && \
    chmod a+x /usr/local/bin/composer && \
    echo "alias composer='php /usr/local/bin/composer'" >> ~/.bashrc && \
    curl -LsS https://phar.phpunit.de/phploc.phar -o /usr/local/bin/phploc && \
    chmod a+x /usr/local/bin/phploc && \
    curl -LsS https://github.com/phpmd/phpmd/releases/download/2.8.2/phpmd.phar -o /usr/local/bin/phpmd && \
    chmod a+x /usr/local/bin/phpmd && \
    curl -LsS https://squizlabs.github.io/PHP_CodeSniffer/phpcs.phar -o /usr/local/bin/phpcs && \
    chmod a+x /usr/local/bin/phpcs && \
    curl -LsS https://squizlabs.github.io/PHP_CodeSniffer/phpcbf.phar -o /usr/local/bin/phpcbf && \
    chmod a+x /usr/local/bin/phpcbf && \
    curl -LsS https://phar.phpunit.de/phpcpd.phar -o /usr/local/bin/phpcpd && \
    chmod a+x /usr/local/bin/phpcpd

# Install Node.js
RUN curl -sL https://deb.nodesource.com/setup_12.x | bash - \
    && apt-get install -y nodejs \
    && apt-get install -y build-essential

USER root

WORKDIR /var/www/html