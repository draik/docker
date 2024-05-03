# syntax=docker/dockerfile:1

FROM wordpress:latest AS wp
COPY .htaccess /var/www/html/.htaccess


FROM mysql:5.7 AS db
ENV MYSQL_RANDOM_ROOT_PASSWORD 1
WORKDIR /var/lib/mysql