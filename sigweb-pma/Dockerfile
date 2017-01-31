FROM phpmyadmin:latest
MAINTAINER Jason Odoom <jo75@njit.edu>

# PHPMyAdmin environment variables
ENV PMA_USER=webdev
ENV PMA_PASSWORD=mIsT3r_tumnus
ENV PMA_ARBITRARY=1

# Add volume for sessions to allow session persistence
VOLUME /sessions

EXPOSE 8080
CMD ["phpmyadmin"] 
