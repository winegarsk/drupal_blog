FROM drupal:8-apache
FROM php:7.4
WORKDIR /

# Install Dependencies
RUN docker-php-ext-install mysqli && docker-php-ext-enable mysqli
RUN apt-get update && apt-get install -y \
	apache2 \
	curl \
	git \
	default-mysql-client \
	vim \
	wget \
	&& rm -rf /var/lib/apt/lists/* \
	&& apt-get clean -y

# Install custom php config
COPY config/php.ini /usr/local/etc/php/

# Add Drush
RUN wget -O drush.phar https://github.com/drush-ops/drush-launcher/releases/download/0.4.2/drush.phar && \
	chmod +x drush.phar && \
	mv drush.phar /usr/local/bin/drush

# Update the default apache site with the config
ADD apache-drupal.conf /etc/apache2/sites-enabled/000-default.conf

# Add src
ADD . /var/www/html/app/

# expose container at port 80
EXPOSE 80

CMD /usr/sbin/apache2ctl -D FOREGROUND
