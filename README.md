# dockerfile-NGINX

FROM debian:buster-slim

MAINTAINER anonymous

WORKDIR /home

RUN apt -y update && apt -y install wget && wget https://nginx.org/download/nginx-1.14.0.tar.gz && tar -xzvf nginx-1.14.0.tar.gz && apt -y install build-essential zlib1g-dev && apt install libpcre3-dev openssl libssl-dev -y && apt install perl libperl-dev libxslt-dev libgd-dev geoip-database libgeoip-dev make -y

COPY http /home/nginx-1.14.0/src/http

WORKDIR /home/nginx-1.14.0/

RUN ./configure --prefix=/etc/nginx --sbin-path=/usr/sbin/nginx --error-log-path=/var/log/nginx/error.log --pid-path=/var/run/nginx.pid --lock-path=/var/run/nginx.lock --user=nginx --group=nginx

RUN make && make install && nginx -v && useradd nginx && chown -R nginx:nginx /etc/nginx/

EXPOSE 80 443

CMD ["nginx", "-g", "daemon off;"]
