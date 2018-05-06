FROM nginx:alpine

RUN apt-get install ruby && gem install rubygems-update && update_rubygems

RUN apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 561F9B9CAC40B2F7

RUN echo "deb https://oss-binaries.phusionpassenger.com/apt/passenger xenial main" >> /etc/apt/sources.list.d/passenger.list

RUN apt-get update && apt-get install nginx-extras passenger

RUN echo "include /etc/nginx/passenger.conf;" >> /etc/nginx/nginx.conf

RUN systemctl restart nginx

RUN usermod -aG gitlab-www www-data