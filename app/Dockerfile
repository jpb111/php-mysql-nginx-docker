FROM php:7.4-fpm

# Copy composer.lock and composer.json
#COPY composer.lock composer.json /var/www/

# Set working directory
WORKDIR /var/www

ARG uid=1000


# Install dependencies
RUN apt-get update && apt-get install -y \
    git \
    zip \
    curl \
    sudo \
    unzip \
    libicu-dev \
    libbz2-dev \
    libpng-dev \
    libjpeg-dev \
    libmcrypt-dev \
    libreadline-dev \
    libfreetype6-dev \
    g++

# Clear cache
RUN apt-get clean && rm -rf /var/lib/apt/lists/*

# Install extensions

RUN docker-php-ext-install \
    bz2 \
    intl \
    bcmath \
    opcache \
    calendar \
    pdo_mysql \
    mysqli


# Install composer
#RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Install Composer 
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

# Add user for codeigniter application
RUN groupadd -g 1000 www
RUN useradd -u 1000 -ms /bin/bash -g www www


# 6. we need a user with the same UID/GID with host user
# so when we execute CLI commands, all the host file's ownership remains intact
# otherwise command from inside container will create root-owned files and directories
#RUN useradd -G www-data,root -u $uid -d /home/devuser devuser
#RUN mkdir -p /home/devuser/.composer && \
   # chown -R devuser:devuser /home/devuser

# Copy existing application directory contents
COPY . /var/www

# Copy existing application directory permissions
COPY --chown=www:www . /var/www

# Change current user to www
USER www

# Expose port 9000 and start php-fpm server
EXPOSE 9000
CMD ["php-fpm"]

# 3. mod_rewrite for URL rewrite and mod_headers for .htaccess extra headers like Access-Control-Allow-Origin-
#RUN a2enmod rewrite headers

# 4. start with base php config, then add extensions
#RUN mv "$PHP_INI_DIR/php.ini-development" "$PHP_INI_DIR/php.ini"






