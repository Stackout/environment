FROM nginx:alpine

RUN apk update \
    && apk add --update \
    && apk add ruby \
    && gem install rubygems-update  --no-document \
    && update_rubygems --no-document

RUN apk add --no-cache gnupg \
    && gpg --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 561F9B9CAC40B2F7

RUN mkdir -p /etc/nginx \
    && mkdir -p /etc/apt/sources.list.d

RUN echo "deb https://oss-binaries.phusionpassenger.com/apt/passenger xenial main" >> /etc/apt/sources.list.d/passenger.list

RUN echo "include /etc/nginx/passenger.conf;" >> /etc/nginx/nginx.conf

RUN systemctl restart nginx

RUN usermod -aG gitlab-www www-data