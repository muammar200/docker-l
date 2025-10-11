# default environment variable = development
ARG ENVIRONMENT=development

#****************************************************************************************************************************************************
#1. Base Image
FROM php:8.2-fpm AS base
# Use the official PHP 8.2 image with FPM (FastCGI Process Manager)

# Install system dependencies + PHP extensions
RUN apt-get update && apt-get install -y \
    git unzip libzip-dev \
    && docker-php-ext-install pdo_mysql zip

# Install Composer
COPY --from=composer:2 /usr/bin/composer /usr/bin/composer

WORKDIR /var/www/html  
# Set working directory to /var/www/html (Laravel or PHP app directory)

# ****************************************************************************************************************************************************

#2. Development Stage
FROM base AS development

# install addtional PHP extensions for development
RUN pecl install xdebug && docker-php-ext-enable xdebug

# # jalankan kalau belum dijalankan sebelumnya
# RUN composer install
# RUN cp .env.example .env 
# RUN php artisan key:generate
# RUN php artisan migrate

CMD ["php-fpm"]  
# Run PHP-FPM as the default container command

#****************************************************************************************************************************************************

#3. Build Stage
FROM base AS build

COPY ./laravel /var/www/html

RUN composer install --no-interaction --optimize-autoloader

RUN chown -R www-data:www-data /var/www/html \
    && chmod -R 755 storage bootstrap/cache
# Set proper permissions for Laravel storage and cache directories

#****************************************************************************************************************************************************

#4. Production Stage
FROM base AS production

COPY --from=build /var/www/html /var/www/html

USER www-data
# Run as non-root user for better security

CMD ["php-fpm"]  
# Run PHP-FPM as the default container command

#****************************************************************************************************************************************************

# 5. Final Stage
FROM ${ENVIRONMENT} AS final