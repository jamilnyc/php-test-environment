FROM php:7.4-apache-buster

# Update and install some common libraries
RUN apt clean
RUN apt update
RUN apt install -qq -y \
    wget \
    apt-utils \
    memcached \
    zip \
    git \
    build-essential \
    libmemcached-dev \
    libz-dev \
    gnupg2 \
    tree \
    vim

# Install Server Components
RUN pecl install memcached \
    && pecl install redis \
    && docker-php-ext-install pdo_mysql \
    && docker-php-ext-install opcache \
    && docker-php-ext-enable memcached \
    && docker-php-ext-enable redis \
	&& docker-php-ext-enable opcache \
    && curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Create directories for logging
RUN mkdir /var/log/my_app \
    && chmod 0777 /var/log/my_app


# You can copy code or configuration files
#COPY . /var/www/
#COPY ./docker/api.conf /etc/apache2/sites-available/

# Install Composer dependencies if any
#RUN chown -R www-data:www-data /var/www \
#    && composer install \
#    && composer dump-autoload --classmap-authoritative


# Enable Apache Mods if needed
#RUN a2dissite 000-default \
#    && a2dissite default-ssl \
#    && a2enmod rewrite \
#    && a2enmod headers \
#    && a2ensite api

# Location of the general PHP INI file and XDebug INI file
ARG PHP_INI=/usr/local/etc/php/php.ini
ARG XDEBUG_INI=/usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini

# Create PHP INI from the development version
RUN mv /usr/local/etc/php/php.ini-development $PHP_INI \
    && sed -i -e 's/memory_limit = .*/memory_limit = 512M/g' $PHP_INI

# Install and configure XDebug for step debugging
RUN /usr/local/bin/pecl install xdebug \
    && /usr/local/bin/docker-php-ext-enable xdebug \
    && /bin/bash -c "echo 'xdebug.mode=debug' >> $XDEBUG_INI" \
    && /bin/bash -c "echo 'xdebug.start_with_request=yes' >> $XDEBUG_INI" \
    && /bin/bash -c "echo 'xdebug.discover_client_host=1' >> $XDEBUG_INI" \
    && /bin/bash -c "echo 'xdebug.client_port=9000' >> $XDEBUG_INI" \
    && /bin/bash -c "echo 'xdebug.client_host=host.docker.internal' >> $XDEBUG_INI" \
    && /bin/bash -c "echo 'xdebug.var_display_max_depth = 6' >> $XDEBUG_INI" \
    && /bin/bash -c "echo 'xdebug.var_display_max_children = 512' >> $XDEBUG_INI" \
    && /bin/bash -c "echo 'xdebug.var_display_max_data = 4096' >> $XDEBUG_INI"

# Enable Opcache if desired
#RUN sed -i -e "s/dirname(dirname(__FILE__))/\/var\/www\/vendor/" /var/www/vendor/composer/autoload_* \
#    && sed -i -e "s/dirname(\$vendorDir)/\/var\/www/" /var/www/vendor/composer/autoload_* \
#    && sed -i -e "s/__DIR__/'\/var\/www\/vendor\/composer'/" /var/www/vendor/composer/*.php \
#	&& sed -i -e "s/;opcache\.enable=.*/opcache.enable=1/g" $PHP_INI \
#	&& sed -i -e "s/;opcache\.revalidate_freq=.*/opcache.revalidate_freq=0/g" $PHP_INI \
#	&& sed -i -e "s/;opcache\.validate_timestamps=.*/opcache.validate_timestamps=0/g" $PHP_INI \
#	&& sed -i -e "s/;opcache\.max_accelerated_files=.*/opcache.max_accelerated_files=7963/g" $PHP_INI \
#	&& sed -i -e "s/;opcache\.memory_consumption=.*/opcache.memory_consumption=192/g" $PHP_INI \
#	&& sed -i -e "s/;opcache\.interned_strings_buffer=.*/opcache.interned_strings_buffer=16/g" $PHP_INI \
#	&& sed -i -e "s/;opcache\.fast_shutdown=.*/opcache.fast_shutdown=1/g" $PHP_INI