FROM php:5.6-apache
MAINTAINER Autoize <info@autoize.com> (@autoize)

# Enable Apache Rewrite Module
RUN a2enmod rewrite

# Install PHP extensions
RUN apt-get update && apt-get install -y libc-client-dev libicu-dev libkrb5-dev libmcrypt-dev libssl-dev unzip zip cron
RUN rm -rf /var/lib/apt/lists/*
RUN docker-php-ext-configure imap --with-imap --with-imap-ssl --with-kerberos
RUN docker-php-ext-install imap intl mbstring mcrypt mysqli pdo pdo_mysql zip

VOLUME /var/www/html

# Define Mautic version and expected SHA1 signature
ENV MAUTIC_VERSION 2.5.1

# Download package and extract to web volume
RUN curl -o mautic.zip -SL https://s3.amazonaws.com/mautic/releases/${MAUTIC_VERSION}.zip \
 && mkdir /usr/src/mautic \
 && unzip mautic.zip -d /usr/src/mautic \
 && rm mautic.zip \
 && chown -R www-data:www-data /usr/src/mautic 

# Copy init scripts and custom .htaccess
COPY docker-entrypoint.sh /entrypoint.sh
COPY makeconfig.php /makeconfig.php
COPY makedb.php /makedb.php

COPY mautic.cron /etc/cron.d/mautic.cron
RUN chmod 644 /etc/cron.d/mautic.cron

RUN touch /var/log/mautic.cron.log
RUN chmod +rw /var/log/mautic.cron.log

RUN /usr/bin/crontab /etc/cron.d/mautic.cron

RUN chmod 755 /entrypoint.sh
ENTRYPOINT ["/entrypoint.sh"]
CMD ["apache2-foreground"]
