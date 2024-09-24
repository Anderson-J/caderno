FROM php:8.2-cli

WORKDIR /var/www

RUN apt update && \
    rm -r /var/www/html && \
    php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" && \
    php composer-setup.php && \
    php -r "unlink('composer-setup.php');" && \
    cp composer.phar /usr/bin/composer && \
    apt update && apt install zip -y && \
    composer global require laravel/installer && \
    composer create-project laravel/laravel example-app && \
    cd example-app;

EXPOSE 8000

ENTRYPOINT ["php","example-app/artisan","serve"]

CMD ["--host=0.0.0.0","--port=8000"]