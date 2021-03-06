FROM php:7-fpm

RUN set -x; \
    apt-get update \
    && apt-get install -y --no-install-recommends \
        postgresql-server-dev-9.4 \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libpng12-dev \
        rsync \
    && rm -r /var/lib/apt/lists/*

RUN docker-php-ext-install mbstring pgsql mysqli \
    && docker-php-ext-configure gd \
        --with-freetype-dir=/usr/include/ \
        --with-jpeg-dir=/usr/include/ \
        --with-png-dir=/usr/include/ \
    && docker-php-ext-install gd

VOLUME /var/www/html

ENV DOTCLEAR_VERSION 2.10.3
ENV DOTCLEAR_DOWNLOAD_URL http://download.dotclear.org/attic/dotclear-${DOTCLEAR_VERSION}.tar.gz
ENV DOTCLEAR_DOWNLOAD_MD5 484fde29d7094485180f732f5c484dd1

RUN mkdir -p /usr/src/dotclear \
    && curl -fsSL -o dotclear.tar.gz "$DOTCLEAR_DOWNLOAD_URL" \
    && echo "$DOTCLEAR_DOWNLOAD_MD5 dotclear.tar.gz" | md5sum -c - \
    && tar -xzf dotclear.tar.gz -C /usr/src/dotclear --strip-components=2 \
    && rm dotclear.tar.gz \
    && chown -R www-data:www-data /usr/src/dotclear \
    && chmod -R 755 /usr/src/dotclear/public /usr/src/dotclear/cache \
    && rm -f /var/www/html/*

ADD docker-entrypoint.sh /entrypoint.sh
ENTRYPOINT ["/entrypoint.sh"]
CMD ["php-fpm"]
