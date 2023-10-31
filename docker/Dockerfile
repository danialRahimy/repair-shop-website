FROM php:8.1.25-apache-bookworm

RUN apt-get update

RUN apt-get install -y curl
RUN apt-get install -y mariadb-client
RUN apt-get install -y unzip
RUN apt-get install -y nano
RUN apt-get install -y nodejs
RUN apt-get install -y npm

ADD https://github.com/mlocati/docker-php-extension-installer/releases/latest/download/install-php-extensions /usr/local/bin/
RUN chmod +x /usr/local/bin/install-php-extensions

ENV APACHE_DOCUMENT_ROOT /var/www/html/public
RUN sed -ri -e 's!/var/www/html!${APACHE_DOCUMENT_ROOT}!g' /etc/apache2/sites-available/*.conf
RUN sed -ri -e 's!/var/www/!${APACHE_DOCUMENT_ROOT}!g' /etc/apache2/apache2.conf /etc/apache2/conf-available/*.conf

RUN sed -ri -e '/SSLCertificateFile.*snakeoil\.pem/c\SSLCertificateFile \/etc\/ssl\/certs\/myCert.crt' /etc/apache2/sites-available/default-ssl.conf
RUN sed -ri -e '/SSLCertificateKeyFile.*snakeoil\.key/cSSLCertificateKeyFile /etc/ssl/private/myCert.key\' /etc/apache2/sites-available/default-ssl.conf
RUN a2enmod rewrite negotiation ssl socache_shmcb
RUN a2ensite default-ssl

COPY --from=composer /usr/bin/composer /usr/bin/composer

RUN echo expose_php=off >> /usr/local/etc/php/conf.d/php.ini

RUN apt-get install -y cron
COPY ./volumes/crontabFiles/crontab /etc/crontab
RUN crontab /etc/crontab
RUN sed -i 's/^exec /service cron start\n\nexec /' /usr/local/bin/apache2-foreground

RUN install-php-extensions ctype
RUN install-php-extensions curl
RUN install-php-extensions dom
RUN install-php-extensions fileinfo
RUN install-php-extensions filter
RUN install-php-extensions hash
RUN install-php-extensions mbstring
RUN install-php-extensions openssl
RUN install-php-extensions pcre
RUN install-php-extensions pdo
RUN install-php-extensions session
RUN install-php-extensions tokenizer
RUN install-php-extensions xml
RUN install-php-extensions pdo_mysql
RUN install-php-extensions zip
RUN install-php-extensions gd
RUN install-php-extensions exif
RUN install-php-extensions sodium
RUN install-php-extensions intl
