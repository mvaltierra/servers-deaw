FROM ubuntu:22.04

RUN apt-get update \
    && apt-get -y upgrade \
    && apt-get install -y apache2 openssl nano 

# Changing directory
WORKDIR /var/www/html

COPY default/. /var/www/html/

# Changing directory
WORKDIR /var/www/

RUN mkdir ./cvaltierra

COPY cvaltierra/. /var/www/cvaltierra/

# Activating ssl module
RUN a2enmod ssl

# Certificates
COPY /certs/apache.crt /etc/ssl/certs/apache.crt
COPY /certs/apache.key /etc/ssl/private/apache.key
COPY /certs/cvaltierra.crt /etc/ssl/certs/cvaltierra.crt
COPY /certs/cvaltierra.key /etc/ssl/private/cvaltierra.key

# Logs Files
WORKDIR /var/log/apache2/

RUN cp /var/log/apache2/error.log /var/log/apache2/error_cvaltierra.log 

RUN cp /var/log/apache2/access.log /var/log/apache2/access_cvaltierra.log

# Changing directory
WORKDIR /etc/apache2/

# Add file user-site
COPY ./000-default.conf /etc/apache2/sites-available/000-default.conf
COPY ./default-ssl.conf /etc/apache2/sites-available/default-ssl.conf
COPY ./cvaltierra.conf /etc/apache2/sites-available/cvaltierra.conf

# Deactivating site
RUN a2dissite 000-default.conf 

# Activating sites
RUN a2ensite 000-default.conf \
    && a2ensite default-ssl.conf \
    && a2ensite cvaltierra.conf

EXPOSE 80 443