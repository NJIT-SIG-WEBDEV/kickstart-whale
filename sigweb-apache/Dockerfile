# Docker image containing packages for SIGWeb
#
# VERSION   0.0.1
FROM ubuntu:14.04
MAINTAINER Jason Odoom <jo75@njit.edu>

RUN apt-get -q update \
    && apt-get -qy --force-yes dist-upgrade \
    && apt-get install -qy apache2 libapache2-mod-auth-mysql php5-mysql \ 
    php5 libapache2-mod-php5 php5-mcrypt 

VOLUME ["/var/www/html", "/var/log/apache2"]

EXPOSE 80 443

WORKDIR /var/www/html

ENTRYPOINT ["apache2ctl", "-D", "FOREGROUND"]
