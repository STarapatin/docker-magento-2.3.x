FROM php:7.4-fpm

ARG USER_ID
ARG GROUP_ID

ENV COMPOSER_ALLOW_SUPERUSER 1

# Install nodejs
RUN curl -sL https://deb.nodesource.com/setup_18.x | bash -

# Install dependencies
RUN apt-get update --fix-missing && apt-get install -y \
	curl \
	procps \
	libldap2-dev \
	wget \
	unzip \
	git \
	build-essential \
	libfreetype6-dev \
	libjpeg62-turbo-dev \
	libxslt-dev \
	libicu-dev \
	libmcrypt-dev \
	libxslt1-dev \
	libsodium-dev \
	libpng-dev \
	zlib1g-dev \
	libzip-dev \
	libxml2-dev \
	mediainfo \
	libonig-dev \
	libjpeg-dev \
	libssl-dev \
	libc-client-dev \
	libkrb5-dev \
	libpq-dev \
    python3-pip \
    python-dev-is-python3 \
    cron \
    webp \
	nodejs \
	graphviz \
	python3 \
    optipng \
    jpegoptim \
    jq \
    gifsicle \
	mc \
	sudo


# Configure the gd library
RUN docker-php-ext-configure \
  gd --with-freetype --with-jpeg
# Install required PHP extensions
RUN docker-php-ext-configure imap --with-kerberos --with-imap-ssl
RUN docker-php-ext-configure intl
RUN docker-php-ext-install -j$(nproc) \
	dom \
	gd \
	pdo_mysql \
	mysqli \
	exif \
	imap \
	zip \
	bcmath \
	opcache \ 
	pcntl \
	ldap \
	xmlwriter \
	simplexml \
	sockets \
	mbstring \
	iconv \
	intl \
	xsl \
	soap 

RUN docker-php-source delete

COPY cron /etc/cron.d/cron
RUN crontab /etc/cron.d/cron

RUN yes | pecl install xdebug-2.9.8

RUN chsh -s /bin/bash www-data

# Install composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer --version=2.7.4

# Retrieve php.ini settings
ADD php.ini /usr/local/etc/php/conf.d/40-custom.ini

# Configure permissions
RUN usermod -u ${USER_ID} www-data && groupmod -g ${GROUP_ID} www-data

RUN sed -i /etc/sudoers -re 's/^%sudo.*/%sudo ALL=(ALL:ALL) NOPASSWD: ALL/g'

WORKDIR /var/www

USER "${USER_ID}:${GROUP_ID}"

# CMD ["sh", "-c", "service cron start && php-fpm && cron -f >> /dev/stdout2>&1"]

CMD ["php-fpm"]