# Docker image containing packages for SIGWeb
#
# VERSION   0.0.1
FROM ubuntu:14.04
MAINTAINER Jason Odoom <jo75@njit.edu>

RUN apt-get -q update \
    && apt-get -qy --force-yes dist-upgrade \
    && apt-get install -qy python-pip python-flask python-dev apt-utils  \ 
    ruby git curl default-jre default-jdk openssh-server build-essential


RUN mkdir /var/run/sshd
RUN echo 'root:curiousfaun' | chpasswd

#Installing nodeJS
RUN curl -sL https://deb.nodesource.com/setup_7.x | bash -
RUN apt-get install -y nodejs

#Copy the file of dependencies to tmp
ADD requirements.txt /tmp

RUN npm install --no-bin-links
RUN npm rebuild node-sass -no-bin-links

RUN pip install -r /tmp/requirements.txt

RUN npm install socket.io 
RUN npm install webpack
RUN gem install sass
RUN npm install --save-dev babelify

#Creating user webdev and setting user passwd
RUN useradd webdev && echo 'webdev:webdev' | chpasswd
RUN echo "webdev ALL=(ALL) NOPASSWD: ALL" >> /etc/sudoers


RUN sed -i 's/UsePAM yes/UsePAM no/g' /etc/ssh/sshd_config

# SSH login fix. Otherwise user is kicked off after login
RUN sed 's@session\s*required\s*pam_loginuid.so@session optional pam_loginuid.so@g' -i /etc/pam.d/sshd

ENV NOTVISIBLE "in users profile"

RUN echo "export TERM=xterm" >> /etc/profile

## Suppress error message 'Could not load host key: ...'
RUN rm /etc/ssh/ssh_host*
RUN /usr/bin/ssh-keygen -t rsa -f /etc/ssh/ssh_host_rsa_key -C '' -N ''
#RUN ssh-keygen -P "" -t dsa -f /etc/ssh/ssh_host_dsa_key


USER webdev

VOLUME ["/home/webdev/"]

EXPOSE 22

CMD ["/usr/sbin/sshd", "-D"]
