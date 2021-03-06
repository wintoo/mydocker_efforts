#Download base image ubuntu 16.04

FROM ubuntu:16.04

#Update the Ubuntu software repository inside the dockerfile with the 'RUN' command.
# Update Ubuntu Software repository

RUN apt-get update

# Then install the applications that we need for the custom image. Install Nginx, PHP-FPM and Supervisord from the Ubuntu repository with apt. Add the RUN commands for Nginx and PHP-FPM installation.
# Install nginx, php-fpm and supervisord from ubuntu repository


RUN apt-get install -y nginx php7.0-fpm supervisor && \
    rm -rf /var/lib/apt/lists/*

#At this stage, all applications are installed and we need to configure them. We will configure Nginx for handling PHP applications by editing the default virtual host configuration. We can replace it our 
### new configuration file, or we can edit the existing configuration file with the 'sed' command.
#In this tutorial, we will replace the default virtual host configuration with a new configuration by using the 'COPY' dockerfile command.
#Define the ENV variable


ENV nginx_vhost /etc/nginx/sites-available/default
ENV php_conf /etc/php/7.0/fpm/php.ini
ENV nginx_conf /etc/nginx/nginx.conf
ENV supervisor_conf /etc/supervisor/supervisord.conf

# Enable php-fpm on nginx virtualhost configuration


COPY default ${nginx_vhost}
RUN sed -i -e 's/;cgi.fix_pathinfo=1/cgi.fix_pathinfo=0/g' ${php_conf} && \
    echo "\ndaemon off;" >> ${nginx_conf}

#Next, configure Supervisord for Nginx and PHP-FPM. We will replace the default Supervisord configuration with a new configuration by using the 'COPY' command.
#Copy supervisor configuration


COPY supervisord.conf ${supervisor_conf}


#Now create a new directory for the php-fpm sock file and change the owner of the /var/www/html directory and PHP directory to www-data.


RUN mkdir -p /run/php && \
    chown -R www-data:www-data /var/www/html && \
    chown -R www-data:www-data /run/php

#Next, define the volume so we can mount the directories listed below to the host machine.
# Volume configuration


VOLUME ["/etc/nginx/sites-enabled", "/etc/nginx/certs", "/etc/nginx/conf.d", "/var/log/nginx", "/var/www/html"]

#Finally, setup the default container command 'CMD' and open the port for HTTP and HTTPS. We will create a new start.sh file for default 'CMD' command when container is starting. The file contains the 'supervisord' command, and we will copy the file to the new image with the 'COPY' dockerfile command.
# Configure Services and Port


COPY start.sh /start.sh
CMD ["./start.sh"]

EXPOSE 80 443
