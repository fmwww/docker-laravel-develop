FROM composer as composer
ENTRYPOINT ["composer"]

FROM php:7.2-fpm-alpine as php72

RUN sed -i "s/dl-cdn.alpinelinux.org/mirrors.aliyun.com/g" /etc/apk/repositories \
    && apk add --no-cache autoconf g++ libtool make curl-dev libxml2-dev linux-headers git \
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
    && docker-php-ext-install -j$(nproc) curl

FROM php72 as workspace

RUN apk add zsh openssh \
    && sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"

WORKDIR /www
ARG GIT_NAME
ARG GIT_EMAIL
ENV GIT_NAME=$GIT_NAME GIT_EMAIL=$GIT_EMAIL
COPY --from=composer /usr/bin/composer /usr/bin/composer
RUN apk update \
    && apk add tzdata vim \
    && cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime \
    && echo "Asia/Shanghai" > /etc/timezone \
    && git config --global user.name "${GIT_NAME}" \
    && git config --global user.email "${GIT_EMAIL}" \
    && echo "export PATH=$PATH:/root/.composer/vendor/bin" >> /root/.zshrc
CMD ["/bin/zsh"]