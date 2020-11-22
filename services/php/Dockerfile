FROM composer as composer
ENTRYPOINT ["composer"]

FROM php:7.3-fpm-alpine as php72

RUN sed -i "s/dl-cdn.alpinelinux.org/mirrors.aliyun.com/g" /etc/apk/repositories \
    && apk add --no-cache autoconf g++ libtool make curl-dev libxml2-dev linux-headers git libpng-dev freetype-dev libjpeg-turbo-dev libzip-dev ffmpeg imagemagick imagemagick-libs imagemagick-dev \
    && echo "---------- Install bcmath ----------" \
    && docker-php-ext-install -j$(nproc) bcmath \
    && echo "---------- Install pdo_mysql ----------" \
    && docker-php-ext-install -j$(nproc) pdo_mysql \
    && echo "---------- Install zip ----------" \
    && docker-php-ext-install -j$(nproc) zip \
    && echo "---------- Install pcntl ----------" \
    && docker-php-ext-install -j$(nproc) pcntl \
    && echo "---------- Install mysqli ----------" \
    && docker-php-ext-install -j$(nproc) mysqli \
    && echo "---------- Install mbstring ----------" \
    && docker-php-ext-install -j$(nproc) mbstring \
    && echo "---------- Install exif ----------" \
    && docker-php-ext-install -j$(nproc) exif \
    && echo "---------- Install bz2 ----------" \
    && apk add --no-cache bzip2-dev \
    && docker-php-ext-install -j$(nproc) bz2 \
    && echo "---------- Install curl ----------" \
    && docker-php-ext-install -j$(nproc) curl \
    && echo "---------- Install gd ----------" \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ --with-png-dir=/usr/include/ \
    && docker-php-ext-install -j$(nproc) gd \
    && echo "---------- Install redis ----------" \
    && curl -L -o /tmp/redis.tar.gz https://github.com/phpredis/phpredis/archive/5.1.1.tar.gz \
    && tar xfz /tmp/redis.tar.gz \
    && rm -r /tmp/redis.tar.gz \
    && mkdir -p /usr/src/php/ext \
    && mv phpredis-5.1.1 /usr/src/php/ext/redis \
    && docker-php-ext-install -j$(nproc) redis \
    && echo "---------- Install imagick ----------" \
    && export CFLAGS="$PHP_CFLAGS" CPPFLAGS="$PHP_CPPFLAGS" LDFLAGS="$PHP_LDFLAGS" \
    && rm -rf /var/lib/apt/lists/* \
    && pecl install imagick-3.4.3 \
    && docker-php-ext-enable imagick

FROM php72 as workspace

ADD zsh.sh install.sh
RUN apk add zsh openssh \
    && chmod +x install.sh && sh -c ./install.sh

WORKDIR /www
ARG GIT_NAME
ARG GIT_EMAIL
ENV GIT_NAME=$GIT_NAME GIT_EMAIL=$GIT_EMAIL
COPY --from=composer /usr/bin/composer /usr/bin/composer
RUN apk update \
    && apk add tzdata vim nodejs npm \
    && cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime \
    && echo "Asia/Shanghai" > /etc/timezone \
    && git config --global user.name "${GIT_NAME}" \
    && git config --global user.email "${GIT_EMAIL}" \
    && echo "export PATH=$PATH:/root/.composer/vendor/bin" >> /root/.zshrc
CMD ["/bin/zsh"]
