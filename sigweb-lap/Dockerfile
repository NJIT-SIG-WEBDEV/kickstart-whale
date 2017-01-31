FROM php:7.0-apache
MAINTAINER Jason Odoom <jo75@njit.edu>

VOLUME ["/var/www/html", "/var/log/apache2", "/usr/local/etc/php/"]

EXPOSE 80 443

WORKDIR /var/www/html

ENTRYPOINT ["apache2ctl", "-D", "FOREGROUND"]
