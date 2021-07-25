FROM php:8-apache
RUN apt-get update && \
    apt-get upgrade -y

# php and supporting system base
RUN apt-get install --fix-missing -y libpq-dev
RUN apt-get install -y curl
RUN apt-get install -y build-essential libssl-dev zlib1g-dev libpng-dev libjpeg-dev libfreetype6-dev
RUN docker-php-ext-install mysqli pdo pdo_mysql zip mbstring
RUN docker-php-ext-enable mysqli
RUN docker-php-ext-enable pdo
RUN docker-php-ext-enable pdo_mysql
RUN apt-get -y install --fix-missing zip unzip
RUN apt-get -y install --fix-missing git

# Composer
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer
RUN composer self-update --2

RUN rm /etc/apache2/sites-available/000-default.conf
COPY sites-available/000-default.conf /etc/apache2/sites-available/000-default.conf

RUN a2enmod rewrite

RUN docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install gd

# Pull from Git Repositories
RUN git clone https://github.com/koetjeengdjalanan/kidsrepublic_Admin.git /var/www/html/

RUN service apache2 restart
