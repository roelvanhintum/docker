ARG PHP_VERSION
FROM craftcms/php-fpm:${PHP_VERSION}

ARG NGINX_CONF=default.conf

# install supervisor and nginx
USER root
RUN apk update --no-cache && apk add --no-cache supervisor nginx
COPY supervisor.conf /etc/supervisor/conf.d/supervisor.conf
COPY nginx.conf /etc/nginx/nginx.conf
COPY craftcms/general.conf /etc/nginx/craftcms/
COPY craftcms/php_fastcgi.conf /etc/nginx/craftcms/
COPY craftcms/security.conf /etc/nginx/craftcms/
COPY ${NGINX_CONF} /etc/nginx/conf.d/default.conf
RUN touch /run/nginx.pid \
    && touch /run/supervisord.pid \
    && chown www-data /run/nginx.pid \
    && chown www-data /run/supervisord.pid \
    && chown -RL www-data:www-data /var/lib/nginx/
USER www-data

EXPOSE 8080
ENTRYPOINT ["/usr/bin/supervisord", "-c", "/etc/supervisor/conf.d/supervisor.conf"]

# ONBUILD COPY --chown=www-data:www-data . .
