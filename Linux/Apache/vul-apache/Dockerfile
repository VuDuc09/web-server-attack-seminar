FROM httpd:2.4.49

COPY ./httpd.no-cgid.conf /usr/local/apache2/conf/httpd.conf

WORKDIR /usr/local/apache2/htdocs/
COPY ./src .

RUN apachectl restart

RUN apt update

RUN echo "F1301{@pache2_ư3b_s3rv3r_vuln_1337}" >> /etc/passwd
