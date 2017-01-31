FROM ubuntu:14.04
MAINTAINER Jason Odoom <jo75@njit.edu>

RUN apt-get -q update \
    && apt-get -qy --force-yes dist-upgrade \
    && apt-get install -qy python-pip python-flask python-dev apt-utils  \ 
    ruby git curl default-jre default-jdk openssh-server build-essential \
    vim python-virtualenv lynx mc 


RUN mkdir /var/run/sshd

RUN echo 'root:curiousfaun' | chpasswd

# installing nodeJS
RUN curl -sL https://deb.nodesource.com/setup_7.x | bash -
RUN apt-get install -y nodejs

# copying the file of dependencies to /tmp
COPY requirements.txt /tmp

RUN npm install --no-bin-links
RUN npm rebuild node-sass -no-bin-links

RUN pip install -r /tmp/requirements.txt

RUN npm install socket.io 
RUN npm install webpack
RUN gem install sass
RUN npm install --save-dev babelify

# creating user webdev and setting user passwd
RUN useradd webdev && echo 'webdev:webdev' | chpasswd
RUN echo "webdev ALL=(ALL) NOPASSWD: ALL" >> /etc/sudoers

RUN sed -i 's/UsePAM yes/UsePAM no/g' /etc/ssh/sshd_config

# configuring supervisor 
RUN apt-get install -qy supervisor

COPY supervisord.conf /etc

ENV HOME /home/webdev
ENV TERM xterm

COPY bash/profile /home/webdev/.profile
COPY bash/bashrc /home/webdev/.bashrc

RUN chown -R webdev:webdev /home/webdev

# let's get a bash shell on login
RUN rm /bin/sh && ln -sf /bin/bash /bin/sh
RUN source ~/.profile 

VOLUME ["/home/webdev/"]

EXPOSE 22
CMD ["/usr/bin/supervisord", "-n", "-c", "/etc/supervisord.conf"]
