FROM php:5.6-fpm

# extions

# Install Core extension
#
# bcmath bz2 calendar ctype curl dba dom enchant exif fileinfo filter ftp gd gettext gmp hash iconv
# imap interbase intl json ldap mbstring mcrypt mssql mysql mysqli oci8 odbc opcache pcntl
# pdo pdo_dblib pdo_firebird pdo_mysql pdo_oci pdo_odbc pdo_pgsql pdo_sqlite pgsql phar posix
# pspell readline recode reflection session shmop simplexml snmp soap sockets spl standard
# sybase_ct sysvmsg sysvsem sysvshm tidy tokenizer wddx xml xmlreader xmlrpc xmlwriter xsl zip
#
# Must install dependencies for your extensions manually, if need.
RUN \
    export mc="-j$(nproc)" \
    && apt-get update \
    && apt-get install -y \
        # for iconv mcrypt
        libmcrypt-dev \
        #   for gd
        libfreetype6-dev libjpeg62-turbo-dev libpng-dev \
        # for bz2
        libbz2-dev \
        # for enchant
        libenchant-dev \
        # for gmp
        libgmp-dev \
        # for soap wddx xmlrpc tidy xsl
        libxml2-dev libtidy-dev libxslt1-dev \
        # for zip
        libzip-dev \
        # for snmp
        libsnmp-dev snmp \
        # for pgsql pdo_pgsql
        libpq-dev \
        # for pspell
        libpspell-dev \
        # for recode
        librecode-dev \
        # for pdo_firebird
        firebird-dev \
        # for pdo_dblib
        freetds-dev \
        # for ldap
        libldap2-dev \
        # for imap
        libc-client-dev libkrb5-dev \
        # for interbase
        firebird-dev \
        # for intl
        libicu-dev \
        # for gearman
        libgearman-dev \
        # for magick
        libmagickwand-dev \
        # for memcached
        zlib1g-dev libmemcached-dev \
        # for mongodb
        autoconf pkg-config libssl-dev \
        # for odbc pdo_odbc
        unixodbc-dev \


    # for gd
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install $mc gd \
    # for bz2
    && docker-php-ext-install $mc bz2 \
    # for enchant
    && docker-php-ext-install $mc enchant \
    # for gmp
    && ln -s /usr/include/x86_64-linux-gnu/gmp.h /usr/include/gmp.h \
    && docker-php-ext-install $mc gmp \
    # for soap wddx xmlrpc tidy xsl
    && docker-php-ext-install $mc soap wddx xmlrpc tidy xsl \
    # for zip
    && docker-php-ext-install $mc zip \
    # for snmp
    && docker-php-ext-install $mc snmp \
    # for pgsql pdo_pgsql
    && docker-php-ext-install $mc pgsql pdo_pgsql \
    # for pspell
    && docker-php-ext-install $mc pspell \
    # for recode
    && docker-php-ext-install $mc recode \
    # for pdo_firebird
    && docker-php-ext-install $mc pdo_firebird \
    # for pdo_dblib
    && docker-php-ext-configure pdo_dblib --with-libdir=lib/x86_64-linux-gnu \
    && docker-php-ext-install $mc pdo_dblib \
    # for ldap
    && docker-php-ext-configure ldap --with-libdir=lib/x86_64-linux-gnu \
    && docker-php-ext-install $mc ldap \
    # for imap
    && docker-php-ext-configure imap --with-kerberos --with-imap-ssl \
    && docker-php-ext-install $mc imap \
    # for interbase
    && docker-php-ext-install $mc interbase \
    # for intl
    && docker-php-ext-install $mc intl \

    # no dependency extension
    && docker-php-ext-install $mc bcmath \
    && docker-php-ext-install $mc calendar \
    && docker-php-ext-install $mc exif \
    && docker-php-ext-install $mc gettext \
    && docker-php-ext-install $mc sockets \
    && docker-php-ext-install $mc dba \
    && docker-php-ext-install $mc mysqli \
    && docker-php-ext-install $mc pcntl \
    && docker-php-ext-install $mc pdo_mysql \
    && docker-php-ext-install $mc shmop \
    && docker-php-ext-install $mc sysvmsg \
    && docker-php-ext-install $mc sysvsem \
    && docker-php-ext-install $mc sysvshm \

#    # Install PECL extensions

    # 增加 odbc, pdo_odbc 扩展
    && set -ex \
    && docker-php-source extract; \
    { \
         echo '# https://github.com/docker-library/php/issues/103#issuecomment-271413933'; \
         echo 'AC_DEFUN([PHP_ALWAYS_SHARED],[])dnl'; \
         echo; \
         cat /usr/src/php/ext/odbc/config.m4; \
    } > temp.m4 \
    && mv temp.m4 /usr/src/php/ext/odbc/config.m4 \
    && docker-php-ext-configure odbc --with-unixODBC=shared,/usr \
    && docker-php-ext-install $mc odbc \
    && docker-php-ext-configure pdo_odbc --with-pdo-odbc=unixODBC,/usr \
    && docker-php-ext-install $mc pdo_odbc \
    && docker-php-source delete \

    #for opcache php5.6
    docker-php-ext-configure opcache --enable-opcache && docker-php-ext-install $mc opcache \

    && apt-get clean all \
    && rm -rf /var/lib/apt/lists/*  \
    && rm -rf /tmp/* \
    && rm -rf /var/tmp/* \

    # for redis
    && pecl install redis-4.0.1 && docker-php-ext-enable redis \

    # for gearman 5.6
    && pecl install gearman && docker-php-ext-enable gearman \

    # for imagick require PHP version 5.6
    && pecl install imagick-3.4.3 && docker-php-ext-enable imagick \

    # for memcached require PHP version 5.6
    && pecl install memcached-2.2.0 && docker-php-ext-enable memcached \

    # for mcrypt require PHP version 5.6
    && docker-php-ext-install $mc mcrypt \

    # for mongodb 5.6
    && pecl install mongodb-1.2.2 && echo "extension=mongodb.so" >> /usr/local/etc/php/conf.d/mongodb.ini \

    # for xedug php5.6
    && set -ex \
    && docker-php-source extract \
    && curl -fsSL 'https://pecl.php.net/get/xdebug-2.5.5.tgz' -o xdebug-2.5.5.tgz \
    && mkdir xdebug \
    && tar -xf xdebug-2.5.5.tgz -C xdebug --strip-components=1 \
    && ( cd xdebug && phpize && ./configure && make $mc && make install ) \
    && docker-php-ext-enable xdebug \
    && docker-php-source delete \

#    # swoole require php5.6
#    && set -ex \
#    && docker-php-source extract \
#    && curl -fsSL 'https://pecl.php.net/get/swoole-2.0.11.tgz' -o swoole-2.0.11.tgz \
#    && mkdir swoole \
#    && tar -xf swoole-2.0.11.tgz -C swoole --strip-components=1 \
#    && cd swoole && phpize && ./configure && make $mc && make install \
#    && docker-php-ext-enable swoole \
#    && docker-php-source delete \

    && echo 'PHP 5.6 extension installed.'


## install composer
RUN curl -sS https://getcomposer.org/installer | php -- --filename=composer --install-dir=/bin
ENV PATH /root/.composer/vendor/bin:$PATH