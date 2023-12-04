FROM php:8.3

COPY --from=composer:2.6.5 /usr/bin/composer /usr/bin/composer

RUN apt-get update \
    && apt-get install -y \
        git \
    # Install node
    && curl -fsSL https://deb.nodesource.com/setup_20.x | bash - \
    && apt-get install --no-install-recommends -y nodejs \
    && node --version \
    && npm --version

# Install PHP extensions
ADD https://github.com/mlocati/docker-php-extension-installer/releases/latest/download/install-php-extensions /usr/local/bin/
RUN chmod uga+x /usr/local/bin/install-php-extensions && sync \
    && install-php-extensions \
    xdebug \
    zip \
    @composer \
    && rm /usr/local/bin/install-php-extensions

RUN echo "alias jigsaw=./vendor/bin/jigsaw" >> ~/.bashrc && \
    echo "alias compile='./vendor/bin/jigsaw build'" >> ~/.bashrc && \
    /bin/bash -c "source ~/.bashrc"

WORKDIR /var/www/html

COPY entrypoint.sh /var/www/scripts/
ENTRYPOINT [ "bash", "/var/www/scripts/entrypoint.sh" ]
