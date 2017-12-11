FROM php:fpm

LABEL maintainer="victor@timecrash.space"
LABEL version="0.9"
LABEL description="Docker image of Live Helper Chat running over PHP-FPM, http server with cgi pass required."

RUN apt-get update \
	&& DEBIAN_FRONTEND=noninteractive apt-get --no-install-recommends -y install unzip libfreetype6-dev libjpeg-dev libpng-dev 
 
WORKDIR /usr/src/
 
RUN debMultiarch="$(dpkg-architecture --query DEB_BUILD_MULTIARCH)" \
 && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ --with-png-dir=/usr/include/ \
 && docker-php-ext-install gd bcmath mysqli pdo_mysql

ADD https://github.com/remdex/livehelperchat/archive/master.zip /usr/src/lhc.zip
 
WORKDIR /var/www/html
 
RUN unzip -q /usr/src/lhc.zip livehelperchat-master/lhc_web/* -d /usr/src \
	&& rm /usr/src/lhc.zip \
	&& cp -r /usr/src/livehelperchat-master/lhc_web/* /var/www/html \
	&& chown -R www-data:www-data .
	
EXPOSE 9000
 
#COPY docker-entrypoint.sh /entrypoint.sh
#RUN chmod 555 /entrypoint.sh
#ENTRYPOINT [ "/entrypoint.sh" ]

CMD [ "php-fpm" ]
