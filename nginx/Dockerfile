FROM nginx:alpine

RUN sudo apt-get install ruby && gem install rubygems-update && update_rubygems

RUN sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 561F9B9CAC40B2F7

RUN sudo echo "deb https://oss-binaries.phusionpassenger.com/apt/passenger xenial main" >> /etc/apt/sources.list.d/passenger.list

RUN sudo apt-get update && apt-get install nginx-extras passenger

RUN sudo echo "include /etc/nginx/passenger.conf;" >> /etc/nginx/nginx.conf

RUN sudo systemctl restart nginx

RUN sudo usermod -aG gitlab-www www-data