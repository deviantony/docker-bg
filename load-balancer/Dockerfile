FROM eboraas/apache

ADD default-site /etc/apache2/sites-available/default

RUN a2enmod proxy proxy_http proxy_balancer

CMD ["/usr/sbin/apache2ctl", "-D", "FOREGROUND"]
