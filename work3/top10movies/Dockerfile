FROM webdevops/php-nginx:7.4-alpine

# Install Laravel framework system requirements (https://laravel.com/docs/8.x/deployment#optimizing-configuration-loading)
RUN apk add oniguruma-dev postgresql-dev libxml2-dev curl-dev libpng libpng-dev libzip-dev
RUN docker-php-ext-install \
        bcmath \
        ctype \
        fileinfo \
        json \
        mbstring \
        tokenizer \
        xml \
	xmlrpc \
	curl \
	gd \
	opcache \
	soap \
	zip \ 
	intl

# Copy Composer binary from the Composer official Docker image
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

ENV WEB_DOCUMENT_ROOT /app/public
ENV APP_ENV production
WORKDIR /app
COPY .env.example .env
COPY . .

RUN composer install --no-interaction --optimize-autoloader --no-dev

#RUN composer dump-autoload

# Optimizing Configuration loading
RUN php artisan config:cache

# Optimizing Route loading
RUN php artisan route:cache

# Optimizing View loading
RUN php artisan view:cache

RUN php artisan key:generate && php artisan config:clear && php artisan config:cache && php artisan route:clear


RUN chown -R application:application .

