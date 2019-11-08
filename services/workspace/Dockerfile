FROM composer as composer
WORKDIR /www
RUN composer config -g repo.packagist composer https://mirrors.aliyun.com/composer/
ENTRYPOINT ["composer"]

FROM php:7.2-fpm as workspace
RUN apt-get update && apt-get install -y \
            libfreetype6-dev \
            libjpeg62-turbo-dev \
            libpng-dev \
            git \
        && docker-php-ext-install -j$(nproc) iconv \
        && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
        && docker-php-ext-install -j$(nproc) gd \
        && docker-php-ext-install -j$(nproc) pcntl \
        && docker-php-ext-install -j$(nproc) bcmath \
        && docker-php-ext-install -j$(nproc) zip
WORKDIR /www
COPY --from=composer /usr/bin/composer /usr/bin/composer
CMD ["/bin/bash"]