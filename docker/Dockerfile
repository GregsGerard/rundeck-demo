FROM rundeck/rundeck:3.4.9

USER root

RUN apt-get update && apt-get install -y \
    git mysql-client lsb-release apt-transport-https ca-certificates && \
#    wget -O /etc/apt/trusted.gpg.d/php.gpg https://packages.sury.org/php/apt.gpg && \
#    echo "deb https://packages.sury.org/php/ $(lsb_release -sc) main" | sudo tee /etc/apt/sources.list.d/php.list && \
#    apt-get update && apt-get insta
    git clone https://github.com/wako057/linuxinit.git && linuxinit/init.sh

RUN apt-get update && apt-get install --no-install-recommends -y \
            autoconf automake bison libtool openssl pkg-config re2c \
            libssl-dev \
            libcurl4-openssl-dev \
            libxml2-dev \
            libreadline-dev \
            libzip-dev \
            libonig-dev \
            libsodium-dev\
            libsqlite3-dev \
            libbz2-dev \
            zlib1g-dev \
            libmcrypt-dev \
            libpng-dev \
            libjpeg-dev \
            libwebp-dev \
            libgeoip-dev \
            libpq-dev \
            g++ && \
        cd /usr/src && \
        git clone -b PHP-7.4.25 https://github.com/php/php-src && \
        cd /usr/src/php-src && \
        ./buildconf --force && \
        ./configure --prefix=/usr/local/ --with-config-file-path=/etc/php --with-config-file-scan-dir=/etc/php/conf.d \
            --with-pear \
            --enable-fpm \
            --enable-simplexml \
            --enable-bcmath \
            --enable-calendar \
            --enable-ctype \
            --with-curl \
            --enable-dom \
            --enable-fileinfo \
            --enable-filter \
            --enable-hash \
            --enable-gd \
            --with-jpeg \
            --with-bz2 \
            --enable-filter \
            --enable-json \
            --enable-libxml \
            --enable-mbstring \
            --enable-intl \
            --enable-mbstring \
            --enable-simplexml \
            --with-pdo-pgsql \
            --with-pdo-mysql=mysqlnd \
            --with-pdo-sqlite \
            --enable-mysqlnd \
            --enable-shmop \
            --with-zlib \
            --with-mysqli=mysqlnd \
            --with-sqlite3 \
            --enable-opcache \
            --with-openssl \
            --enable-xmlreader \
            --enable-xmlwriter \
            --with-sodium \
            --with-zip \
            --enable-sockets && \
        make && \
        make install

# Install composer
RUN curl -o /tmp/composer-setup.php https://getcomposer.org/installer \
    && curl -o /tmp/composer-setup.sig https://composer.github.io/installer.sig \
    && php -r "if (hash('SHA384', file_get_contents('/tmp/composer-setup.php')) !== trim(file_get_contents('/tmp/composer-setup.sig'))) { unlink('/tmp/composer-setup.php'); echo 'Invalid installer' . PHP_EOL; exit(1); }" \
    && php /tmp/composer-setup.php --install-dir=/usr/local/bin \
    && php -r "unlink('/tmp/composer-setup.php');" \
    && php -r "unlink('/tmp/composer-setup.sig');" \
    && ln -s /usr/local/bin/composer.phar /usr/local/bin/composer

USER rundeck
RUN git clone https://github.com/wako057/linuxinit.git && linuxinit/init.sh
