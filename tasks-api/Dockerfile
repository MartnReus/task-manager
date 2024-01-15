FROM php:8-apache

RUN apt update && apt install git zip unzip libzip-dev -y

RUN sed -i 's!/var/www/html!/var/www/html/public!g' /etc/apache2/sites-available/000-default.conf

RUN curl -sS https://getcomposer.org/installer -o composer-setup.php
RUN php composer-setup.php --install-dir=/usr/local/bin --filename=composer

WORKDIR /var/www/html
COPY composer.json composer.lock /var/www/html

RUN composer install --no-scripts --no-autoloader

COPY . /var/www/html
RUN composer dump-autoload --optimize

RUN chown -R www-data:www-data /var/www/html/public
RUN mkdir -p /var/www/html/storage \
    && mkdir -p /var/www/html/bootstrap/cache \
    && chown -R www-data:www-data /var/www/html/storage \
    && chown -R www-data:www-data /var/www/html/bootstrap/cache

RUN a2ensite 000-default

# Comando para iniciar Apache
CMD ["apache2-foreground"]
