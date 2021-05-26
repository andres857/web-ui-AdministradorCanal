FROM ubuntu:18.04

RUN apt-get update
RUN apt-get install -y build-essential libpcre3 libpcre3-dev libssl-dev git wget tar zlib1g zlib1g-dev

WORKDIR /home
RUN git clone https://github.com/sergey-dryabzhinsky/nginx-rtmp-module.git

RUN wget https://nginx.org/download/nginx-1.18.0.tar.gz && tar -xf nginx-1.18.0.tar.gz
RUN cd nginx-1.18.0 && \
    ./configure \
    --with-http_ssl_module \ 
    --add-module=../nginx-rtmp-module && \
    make && \
    make install

RUN mkdir -p /nginx/hls


EXPOSE 1935
EXPOSE 8080
CMD [ "/usr/local/nginx/sbin/nginx" ,"-g" ,"daemon off;"]

