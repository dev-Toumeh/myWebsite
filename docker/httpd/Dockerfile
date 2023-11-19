FROM php:8.1.25-apache-bullseye
WORKDIR /var/www/t3coredev

# Define a build-time variables
ARG USERNAME=naseem
ARG GID=1000
ARG UID=1000

## Install system dependencies
RUN apt-get update \
    && apt-get install -y \
    libxml2-dev \
    libzip-dev \
    zlib1g-dev \
    libfreetype6-dev \
    libjpeg62-turbo-dev \
    libpng-dev \
    libjpeg-dev \
    libltdl-dev \
    libicu-dev \
    libmemcached-dev \
    libssl-dev \
    libonig-dev \
    iputils-ping \
    vim \
    wget \
    curl \
    git \
    build-essential \
    libcurl4 \
    libcurl4-openssl-dev \
    && rm -rf /var/lib/apt/lists/*

# Install Composer manually
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

## Install PHP extensions
RUN docker-php-ext-configure gd \
    --with-freetype=/usr/include/ \
    --with-jpeg=/usr/include/ \
    && docker-php-ext-install \
    pdo \
    session \
    filter \
    mbstring \
    intl \
    fileinfo \
    gd \
    zip \
    xml \
    dom \
    pdo_mysql \
    opcache

## install ImageMagick https://www.linuxcapable.com/how-to-install-imagemagick-on-debian-linux/
#RUN git clone https://github.com/ImageMagick/ImageMagick.git /usr/local/src/ImageMagick \
#    cd /usr/local/src/ImageMagick \
#    ./configure \
#    make \
#    make install \
#    sudo ldconfig /usr/local/lib

RUN a2enmod alias \
    && a2enmod authz_core \
    && a2enmod deflate \
    && a2enmod expires \
    && a2enmod filter \
    && a2enmod headers \
    && a2enmod rewrite \
    && a2enmod setenvif

# Create a user with UID 1000 and GID 1000
RUN groupadd -g ${GID} ${USERNAME} \
    && useradd -r -u ${UID} -g ${USERNAME} ${USERNAME}

# change the owner of some directories for development purposes
RUN chown -R ${USERNAME}:${USERNAME} /etc/apache2 /var/lib/apache2 /var/www
