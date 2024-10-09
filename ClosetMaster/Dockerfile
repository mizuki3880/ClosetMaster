# ベースイメージを指定
FROM php:8.2-fpm

# 必要なパッケージをインストール
RUN apt-get update && apt-get install -y \
    libpng-dev \
    libjpeg-dev \
    libfreetype6-dev \
    zip \
    unzip \
    git \
    curl \
    && docker-php-ext-configure gd --with-freetype --with-jpeg \
    && docker-php-ext-install gd \
    && docker-php-ext-install pdo_mysql

# Composerをインストール
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

# Node.js 18.xをインストール
RUN curl -sL https://deb.nodesource.com/setup_18.x | bash - \
    && apt-get install -y nodejs

# 作業ディレクトリを作成
WORKDIR /var/www

# Laravelプロジェクトをコピー
COPY . .

# Composerの依存関係をインストール
RUN composer install --no-interaction --prefer-dist --optimize-autoloader

# npmの依存関係をインストール
RUN npm install

# Laravelのキャッシュをクリア
RUN php artisan config:clear

# npmのビルドを実行
RUN npm run build

# ポートを公開
EXPOSE 8000

# アプリケーションを起動
CMD ["php", "artisan", "serve", "--host=0.0.0.0", "--port=8000"]
