FROM php:8.1.1-apache

  # Composer
  ARG composer_ver=2.2.1
  ADD https://github.com/mlocati/docker-php-extension-installer/releases/latest/download/install-php-extensions /usr/local/bin/
  RUN chmod +x /usr/local/bin/install-php-extensions && sync \
    && install-php-extensions @composer-${composer_ver}

  # Xdebug
  RUN pecl install xdebug \
  && docker-php-ext-enable xdebug
  
  # その他パッケージ、PHP拡張
  RUN apt-get update \
    # unzip (composer create-project で必要)
    && apt-get install -y unzip \
    # Vim 
    && apt-get install -y vim \
    # PHP BCMath
    && docker-php-ext-install bcmath \
    # PHP GD
    && apt-get install -y zlib1g-dev libpng-dev libjpeg62-turbo-dev \
    && docker-php-ext-configure gd --with-jpeg \
    && docker-php-ext-install -j$(nproc) gd \
    # PHP intl
    && apt-get install -y libicu-dev \
    && docker-php-ext-install intl \
    # PHP PDO MySQL
    && docker-php-ext-install pdo_mysql \
    # mod_rewrite 有効化
    && a2enmod rewrite


    # Apache
    ## https化
    COPY ssl/server.crt /etc/ssl/certs/
    COPY ssl/server.key /etc/ssl/private/
    RUN sed -i 's!/etc/ssl/certs/ssl-cert-snakeoil.pem!/etc/ssl/certs/server.crt!g' /etc/apache2/sites-available/default-ssl.conf \
        && sed -i 's!/etc/ssl/private/ssl-cert-snakeoil.key!/etc/ssl/private/server.key!g' /etc/apache2/sites-available/default-ssl.conf

    ## Document Root.
    RUN sed -i 's!/var/www/html!/var/www/html!g' /etc/apache2/sites-available/default-ssl.conf

    ## 設定を適用
    RUN a2enmod ssl \
        && a2ensite default-ssl.conf
