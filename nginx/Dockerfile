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

# passenger

ENV PASSENGER_VERSION 5.0.30

ENV PATH "/opt/passenger/bin:$PATH"

RUN echo "http://alpine.gliderlabs.com/alpine/edge/main" > /etc/apk/repositories \
    \
    && apk add --no-cache --virtual .passenger-rundeps ca-certificates ruby procps curl pcre libstdc++ libexecinfo \
    && update-ca-certificates \
    && apk add --no-cache --virtual .passenger-builddeps build-base ruby-dev linux-headers curl-dev pcre-dev libexecinfo-dev \
    \
    && mkdir -p /opt \
    && curl -sSL https://github.com/phusion/passenger/archive/release-${PASSENGER_VERSION}.tar.gz | tar -zx -C /opt \
    && mv /opt/passenger-release-${PASSENGER_VERSION} /opt/passenger \
    \
    && export EXTRA_PRE_CFLAGS="-O" \
    && export EXTRA_PRE_CXXFLAGS="-O" \
    && export EXTRA_LDFLAGS="-lexecinfo" \
    \
    && passenger-config compile-agent --auto --optimize \
    && passenger-config install-standalone-runtime --auto --url-root=fake --connect-timeout=1 \
    && passenger-config build-native-support \
    \
    && mkdir -p /usr/src/app \
    \
    && rm -rf /tmp/* \
    \
    && mv /opt/passenger/src/ruby_supportlib /tmp \
    && mv /opt/passenger/src/nodejs_supportlib /tmp \
    && mv /opt/passenger/src/ruby_native_extension /tmp \
    && mv /opt/passenger/src/helper-scripts /tmp \
    && rm -rf /opt/passenger/src/* \
    \
    && mv /tmp/* /opt/passenger/src/ \
    \
    && passenger-config validate-install --auto \
    && apk del .passenger-builddeps \
    && rm -rf /usr/include/execinfo.h \
        /opt/passenger/doc

RUN rm -rf \
    /var/cache/apk/* \
    /tmp/* \
    /etc/ssl \
    /root/.gnupg \
    /usr/share/man



RUN echo "include /etc/nginx/passenger.conf;" >> /etc/nginx/nginx.conf

RUN systemctl restart nginx

RUN usermod -aG gitlab-www www-data