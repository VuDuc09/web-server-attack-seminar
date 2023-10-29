FROM nginx:1.17.8-alpine

WORKDIR /var/www/html

COPY ./web .
COPY ./config/default.conf /etc/nginx/conf.d/default.conf

RUN PATH=/sbin:/bin:/usr/sbin:/usr/bin:/usr/local/sbin:/usr/local/bin

# Grant permission
RUN chmod 777 /var/www/html/uploadfiles