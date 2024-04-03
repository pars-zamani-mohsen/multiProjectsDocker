FROM php:8.1-fpm as php

# Install system dependencies
RUN apt-get update && apt-get install -y \
    git \
    curl \
    libpng-dev \
    libonig-dev \
    libxml2-dev \
    libzip-dev \
    libicu-dev \
    libsqlite3-dev \
    sqlite3 \
    zip \
    unzip \
    supervisor \
    redis-server \
    && pecl install redis \
    && docker-php-ext-enable redis

# Clear cache
RUN apt-get clean && rm -rf /var/lib/apt/lists/*

# Install PHP extensions
RUN docker-php-ext-install pdo_mysql mbstring exif pcntl bcmath gd xml zip soap intl pdo_sqlite

# Add to ini file
RUN echo "extension=bcmath.so" > /usr/local/etc/php/conf.d/docker-php-ext-bcmath.ini && \
    echo "extension=exif.so" > /usr/local/etc/php/conf.d/docker-php-ext-exif.ini && \
    echo "extension=gd.so" > /usr/local/etc/php/conf.d/docker-php-ext-gd.ini && \
    echo "extension=pcntl.so" > /usr/local/etc/php/conf.d/docker-php-ext-pcntl.ini && \
    echo "extension=pdo_mysql.so" > /usr/local/etc/php/conf.d/docker-php-ext-pdo_mysql.ini && \
    echo "extension=redis.so" > /usr/local/etc/php/conf.d/docker-php-ext-redis.ini && \
    echo "extension=soap.so" > /usr/local/etc/php/conf.d/docker-php-ext-soap.ini && \
    echo "extension=sodium.so" > /usr/local/etc/php/conf.d/docker-php-ext-sodium.ini && \
    echo "extension=zip.so" > /usr/local/etc/php/conf.d/docker-php-ext-zip.ini


# Install Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Set working directory
WORKDIR /var/www/html

# Copy existing application directory contents
COPY ./ /var/www/html


#Install dependencies
#RUN composer install  --working-dir=/var/www/html

# Copy supervisor config and entrypoint script
COPY supervisord.conf /etc/supervisor/conf.d/supervisord.conf
COPY entrypoint.sh /usr/local/bin/entrypoint.sh


# Make entrypoint script executable and run it on container start-up.
RUN chmod +x /usr/local/bin/entrypoint.sh

CMD ["/usr/local/bin/entrypoint.sh"]
