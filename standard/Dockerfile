FROM davyinsa/docker-nginx-php:8.2-alpine
ENV DRUPAL_WEB_ROOT=web
ENV PHP_ENABLE_SQLITE=TRUE
ENV PHP_FPM_PROCESS_MANAGER=ondemand
ENV PHP_FPM_MAX_CHILDREN=5
COPY --from=drupal:10 /opt/drupal /var/www/html

RUN apk add --no-cache sqlite && \
    php-ext enable pdo_sqlite && \
    cd /var/www/html && \
    # rm -rf vendor && rm composer.lock && \
    composer install && \
    #mkdir -p ~/.config/fish/completions/ && ln -s ~/.console/drupal.fish ~/.config/fish/completions/drupal.fish && \
    chown nginx:www-data /var/www/html && \
    ## install drupal module
    sed -i 's/"minimum-stability": "stable"/"minimum-stability": "dev"/g' composer.json && \
    composer require drush/drush drupal/admin_toolbar drupal/devel drupal/devel_php davyin/drupal_settings && \
    cp vendor/davyin/drupal_settings/assets/* web/sites/default/ && \
    cp web/sites/default/default.settings.php web/sites/default/settings.php && \
    chmod 777 web/sites/default/settings.php && \
    composer drupal:scaffold -q && \
    mkdir -p web/sites/default/files && \
    chmod -R 777 web/sites/default/files