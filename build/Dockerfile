FROM php:7.4-alpine

MAINTAINER toph <toph@toph.fr>

ENV COMPOSER_VERSION=1.10.8
RUN export COMPOSER_HOME=/tmp/composer \
    && cd /usr/local/bin \
    && EXPECTED_SIGNATURE=$(curl -sSL https://composer.github.io/installer.sig) \
    && php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" \
    && ACTUAL_SIGNATURE=$(php -r "echo hash_file('SHA384', 'composer-setup.php');") \
    && if [ "$EXPECTED_SIGNATURE" != "$ACTUAL_SIGNATURE" ]; then >&2 echo 'ERROR: Invalid installer signature'; exit 1; fi \
    && php composer-setup.php --version=$COMPOSER_VERSION \
    && php -r "unlink('composer-setup.php');" \
    && ln -s composer.phar composer \
    && rm -Rf $COMPOSER_HOME

ENV COMPOSER_ALLOW_SUPERUSER 1
ENV COMPOSER_HOME /usr/local/lib/composer
ENV COMPOSER_BIN_DIR /usr/local/bin

RUN composer global init -s stable -q \
    && composer global require -n pheromone/phpcs-security-audit \
    && phpcs --config-set installed_paths $COMPOSER_HOME/vendor/pheromone/phpcs-security-audit \
    && composer global clear-cache -n

RUN echo "memory_limit=-1" > /usr/local/etc/php/conf.d/zzz.ini

ENTRYPOINT ["/usr/local/bin/phpcs", "--standard=/usr/local/lib/composer/vendor/pheromone/phpcs-security-audit/example_base_ruleset.xml"]
