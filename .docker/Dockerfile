# Use an official PHP 8.1 image as the base image
FROM php:8.1-apache

# Install system dependencies and PHP extensions
RUN apt-get update && \
  apt-get install -y \
  libpng-dev \
  libjpeg-dev \
  libfreetype6-dev \
  libzip-dev \
  zip \
  unzip \
  && docker-php-ext-configure gd --with-freetype --with-jpeg \
  && docker-php-ext-install -j$(nproc) gd pdo pdo_mysql zip

# Enable Apache modules and rewrite module for Laravel's .htaccess
RUN a2enmod rewrite

WORKDIR /var/www/html

# Set the document root to Laravel's public directory
ENV APACHE_DOCUMENT_ROOT /var/www/html/public

# Enable PHP error logging to stdout
RUN ln -sf /dev/stdout /var/log/apache2/error.log

# Copy the Laravel project files into the container
COPY ./web /var/www/html

COPY .docker/laravel-test.conf /etc/apache2/sites-available/laravel-test.conf

RUN a2ensite laravel-test.conf

# Install Composer (PHP package manager) globally
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer


# # Install Laravel dependencies using Composer
RUN composer install

# Set the appropriate permissions for Laravel
RUN chown -R www-data:www-data /var/www/html/storage /var/www/html/bootstrap/cache

# Expose port 80 for Apache
EXPOSE 80

# Start the Apache web server
CMD ["apache2-foreground"]
