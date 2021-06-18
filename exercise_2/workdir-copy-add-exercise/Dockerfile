# WORKDIR, COPY and ADD example
FROM ubuntu:latest
RUN apt-get update && apt-get install apache2 -y
WORKDIR /var/www/html/
COPY index.html .
ADD https://www.docker.com/site/default/files/d8/2019-07/Moby-log.png ./logo.png
CMD ["ls"]