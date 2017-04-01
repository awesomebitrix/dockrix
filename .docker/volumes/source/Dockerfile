FROM debian:jessie

MAINTAINER Igor Barkowsky <igor.barkowsky@gmail.com>

WORKDIR /var/www/site

#ADD http://www.1c-bitrix.ru/download/scripts/bitrixsetup.php /var/www/site
RUN chown -R www-data:www-data /var/www/site/ && chmod -R 775 /var/www/site

#CMD ["true"]