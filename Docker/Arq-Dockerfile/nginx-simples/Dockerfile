FROM nginx:latest AS base
LABEL maintainer="Rafael Goncalves"

WORKDIR /app
RUN set -x \
    echo -e "Processo de build - WebBrowser\n" \
    apt update \
    apt install nginx htop iputils-ping -y 

COPY config/nginx.conf /etc/nginx/nginx.conf
COPY config/conf.d/default.conf /etc/nginx/conf.d/default.conf
COPY app/* /usr/share/nginx/html

RUN set -x \ 
    echo "nginx:${TAG}" >> /usr/share/nginx/html/version.html

EXPOSE 80
# EXPOSE 443

STOPSIGNAL SIGTERM

CMD ["nginx", "-g", "daemon off;"]
