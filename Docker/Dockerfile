FROM php:7.4-apache AS builder

# Install extensions
RUN apt-get update && apt-get install -y \
    libfreetype6-dev \
    libjpeg62-turbo-dev \
    libpng-dev \
    libpq-dev \
    net-tools \
    git \
    && docker-php-ext-install -j$(nproc) iconv \
    && docker-php-ext-configure gd --with-freetype=/usr/include/ --with-jpeg=/usr/include/ \
    && docker-php-ext-configure pgsql -with-pgsql=/usr/local/pgsql \
    && docker-php-ext-install -j$(nproc) gd pdo pdo_mysql pdo_pgsql pgsql

FROM builder AS build1
RUN mkdir -p /var/www/html/speedtest/
RUN git clone https://github.com/fenar/speedtest
# Copy sources

FROM build1 AS final
COPY speedtest/speedtest_worker.min.js /var/www/html/
COPY speedtest/garbage.php /var/www/html/
COPY speedtest/getIP.php /var/www/html/
COPY speedtest/empty.php /var/www/html/
COPY speedtest/example-pretty.html /var/www/html/index.html
RUN chown -R www-data /var/www/html/*

WORKDIR /var/www/html/

EXPOSE 80
