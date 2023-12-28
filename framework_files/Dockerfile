## Use the base image with PHP and Apache
FROM php:8.2-apache

# Set the working directory in the container
WORKDIR /var/www/html

# Enable Apache modules
RUN a2enmod rewrite

# Update package lists and install required dependencies
RUN apt-get update && apt-get install -y \
    zlib1g-dev \
    libicu-dev \
    g++ \
    git \
    unzip 

# Install the PHP intl extension
RUN docker-php-ext-configure intl && \
    docker-php-ext-install intl

# Copy the application files into the container
COPY app_files/ /var/www/html/

# Install Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Run Composer to install dependencies
RUN composer install --no-scripts --no-autoloader

# Clear cache and configuration (if applicable in your CodeIgniter project)
# Replace these commands with the appropriate CodeIgniter 4 commands if needed
RUN php spark cache:clear
RUN php spark config:clear

# Generate the application key (if required in your CodeIgniter project)
RUN php spark key:generate

# Set proper permissions if needed
RUN chown -R www-data:www-data /var/www/html

# Configure Apache within the Dockerfile
RUN echo "ServerName localhost" >> /etc/apache2/apache2.conf
RUN sed -ri -e 's!/var/www/html!/var/www/html/public!g' /etc/apache2/sites-available/*.conf \
    && sed -ri -e 's!/var/www/!/var/www/html/public!g' /etc/apache2/apache2.conf /etc/apache2/conf-available/*.conf

# Expose port 8080 inside the container
EXPOSE 8080

# Start Apache
CMD ["apache2-foreground"]
