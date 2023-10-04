FROM php:8.2

# Update packages
RUN apt-get update

# Install Git
RUN apt-get install git -y

# Install Composer
RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" \
    && php -r "if (hash_file('sha384', 'composer-setup.php') === 'e21205b207c3ff031906575712edab6f13eb0b361f2085f1f1237b7126d785e826a450292b6cfd1d64d92e6563bbde02') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;" \
    && php composer-setup.php \
    && php -r "unlink('composer-setup.php');" \
    && mv composer.phar /usr/local/bin/composer

# Setup /chirper as the workdir with src
COPY ./chirper /chirper/
WORKDIR /chirper

# Install composer packages
RUN composer install

# Install php mysql driver
RUN docker-php-ext-install mysqli pdo pdo_mysql && docker-php-ext-enable pdo_mysql

# Start laravel server
CMD [ "php", "artisan", "serve", "--host=0.0.0.0" ]