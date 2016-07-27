FROM php:5.6-apache

MAINTAINER Carw <carw.master@gmail.com>

RUN apt-get update && apt-get install -y \
    libicu-dev \
    libpq-dev \
    libmemcached-dev \
    curl \
    libpng12-dev \
    libfreetype6-dev \
    libssl-dev \
    libmcrypt-dev \
    --no-install-recommends \
    && rm -r /var/lib/apt/lists/*

# configure gd library
RUN docker-php-ext-configure gd \
    --enable-gd-native-ttf \
    --with-freetype-dir=/usr/include/freetype2

# Install extensions using the helper script provided by the base image
RUN docker-php-ext-install \
    pdo_mysql \
    pdo_pgsql \
#    mysqli \
    gd \
    intl \
    opcache

# Install memcached
RUN pecl install memcached \
    && docker-php-ext-enable memcached

# APCu
RUN pecl install apcu-4.0.10 \
	&& echo "extension=apcu.so\napc.enable_cli = 1" > /usr/local/etc/php/conf.d/ext-apcu.ini

# imagick
RUN runtimeRequirements="libmagickwand-6.q16-dev --no-install-recommends" \
	&& apt-get update && apt-get install -y ${runtimeRequirements} \
	&& ln -s /usr/lib/x86_64-linux-gnu/ImageMagick-6.8.9/bin-Q16/MagickWand-config /usr/bin/ \
	&& pecl install imagick \
	&& echo "extension=imagick.so" > /usr/local/etc/php/conf.d/ext-imagick.ini \
	&& rm -rf /var/lib/apt/lists/*

# Add custom settings
RUN { \
		echo "expose_php = Off"; \
		echo "upload_max_filesize = 32M"; \
		echo "post_max_size = 32M"; \
		echo "date.timezone = "Asia/Yekaterinburg""; \
		echo 'opcache.memory_consumption=128'; \
		echo 'opcache.interned_strings_buffer=8'; \
		echo 'opcache.max_accelerated_files=4000'; \
		echo 'opcache.revalidate_freq=60'; \
		echo 'opcache.fast_shutdown=1'; \
		echo 'opcache.enable_cli=1'; \
	} > /usr/local/etc/php/conf.d/custom_settings.ini

WORKDIR /var/www/html

EXPOSE 80