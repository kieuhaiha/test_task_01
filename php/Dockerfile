FROM php:8.1-fpm

# 1) Системные зависимости для pdo_pgsql, git, zip
RUN apt-get update && apt-get install -y \
    libpq-dev \
    zip unzip git \
  && docker-php-ext-install pdo_pgsql

# 2) Расширение Redis через PECL
RUN pecl install redis \
  && docker-php-ext-enable redis

# 3) Копируем Composer из официального образа
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

#
# ————— УСТАНОВКА ЗАВИСИМОСТЕЙ —
#

# Рабочая директория — /var/www
WORKDIR /var/www

# Копируем только composer.json и ставим зависимости сюда
COPY composer.json ./
RUN composer install --no-dev --optimize-autoloader

#
# ————— КОПИРУЕМ КОД ПРИЛОЖЕНИЯ —
#

# Теперь переключаемся в /var/www/html
WORKDIR /var/www/html

# Копируем ваш src (index.php и всё прочее) в /var/www/html
COPY src/ ./

# Опционально, выставим права
RUN chown -R www-data:www-data /var/www

# По умолчанию PHP-FPM слушает порт 9000
