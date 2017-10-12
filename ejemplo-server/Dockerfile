FROM centos:6
MAINTAINER Julio Sarango <jsarangoq@gmail.com>
COPY includes/* /includes/ 

#agregamos repositorios
#actualizamos, instalamos vim, apache y php.
#rpm bajados de esta url

# https://dl.fedoraproject.org/pub/epel/epel-release-latest-6.noarch.rpm
# https://mirror.webtatic.com/yum/el6/latest.rpm

RUN rpm -Uvh /includes/epel-release-latest-6.noarch.rpm && \
    rpm -Uvh /includes/latest.rpm && \    
    yum -y install httpd && \
    yum -y groupinstall "Development Tools" && \
    yum -y install vim && \

    yum -y install php56w php56w-opcache php56w-pear php56w-devel zlib zlib-devel bc libaio glibc php56w-soap php56w-pecl-xdebug php56w-devel php56w-pecl-json php56w-pear php56w-cli php56w-gd php56w-xmlrpc php56w-pdo php56w-mysql php56w-pgsql php56w-mcrypt php56w-common php56w-ldap php56w php56w-xml php56w-xdebug && \


#instalamos oci8

    rpm -ivh /includes/oracle-instantclient12.1-basic-12.1.0.2.0-1.x86_64.rpm && \
    rpm -ivh /includes/oracle-instantclient12.1-devel-12.1.0.2.0-1.x86_64.rpm && \

    ln -s /usr/include/oracle/12.1/client64 /usr/include/oracle/12.1/client && \
    ln -s /usr/lib/oracle/12.1/client64 /usr/lib/oracle/12.1/client && \

    touch /etc/profile.d/oracle.sh && \
    echo 'export LD_LIBRARY_PATH=/usr/lib/oracle/12.1/client64/lib' >> /etc/profile.d/oracle.sh && \
    source /etc/profile.d/oracle.sh && \

#INSTALAMOS LA EXTENSION OCI8
    pecl download oci8-1.4.10 && \
    tar -xvf oci8-1.4.10.tgz 

WORKDIR oci8-1.4.10

RUN phpize && \
    ./configure --with-oci8=shared,instantclient,/usr/lib/oracle/12.1/client64/lib && \
    make && \
    make install && \

#habilitamos la extension oci8
    touch /etc/php.d/oci8.ini && \
    echo 'extension=oci8.so' >> /etc/php.d/oci8.ini && \

    chown -R apache:apache /var/www/html/ && \
    chown -R apache:apache /var/lib/php/session && \

    mv /includes/php.ini /etc/ && \
    mv /includes/httpd.conf /etc/httpd/conf/ && \
    mv /includes/run-httpd.sh /run-httpd.sh && \

    rm -rf /includes && \
    
    chmod -v +x /run-httpd.sh

WORKDIR /

CMD ["/run-httpd.sh"]
EXPOSE 80

