FROM magi2c/alpine-php:7

ENV USER root
ENV PASSWORD root

RUN echo "http://dl-4.alpinelinux.org/alpine/edge/testing" >> /etc/apk/repositories

RUN apk add --no-cache \
        openssh \
        supervisor \
        git \
        zip \
        wget \
        curl \
        php7-xdebug \

    && sed -i s/#PermitRootLogin.*/PermitRootLogin\ yes/ /etc/ssh/sshd_config \
    && echo "${USER}:${PASSWORD}" | chpasswd \
    && ssh-keygen -A \

    && rm -rf /var/cache/apk/* \

    && curl -sS https://getcomposer.org/installer | php7 -- --install-dir=/usr/bin --filename=composer \

    && wget https://phar.phpunit.de/phpunit.phar \
    && chmod +x phpunit.phar \
    && mv phpunit.phar /usr/bin/phpunit

COPY config/supervisor/supervisor.conf /etc/supervisor/conf.d/supervisord.conf

ENTRYPOINT ["supervisord", "--nodaemon", "--configuration", "/etc/supervisor/conf.d/supervisord.conf"]


EXPOSE 22 9000
