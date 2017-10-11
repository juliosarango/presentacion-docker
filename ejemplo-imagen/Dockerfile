FROM centos
LABEL maintainer 'julio sarango <jsarangoq@gmail.com>'
RUN yum -y install vim
RUN mkdir config 
ADD ./files/config /config
RUN mkdir usuarios
ADD ./files/usuarios /usuarios
RUN yum -y install emacs
