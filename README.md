# Instalar LEMP stack en WSL2 (Ubuntu)

## Actualizar el systema

```bash
sudo apt update && sudo apt full-upgrade -y
```

## Añadir repositorio para estar siempre a la última

```bash
sudo add-apt-repository ppa:ondrej/nginx -y && \
sudo add-apt-repository ppa:ondrej/php -y && \
sudo apt-key adv --fetch-keys 'https://mariadb.org/mariadb_release_signing_key.asc' && \
sudo add-apt-repository 'deb [arch=amd64,arm64,ppc64el] http://ftp.hosteurope.de/mirror/mariadb.org/repo/10.5/ubuntu focal main'
```

## Actualizar los repositorios y instalar CURL + GIT + ZIP + UNZIP + NGINX + MARIADB + PHP7.4

```bash
sudo apt update && \
sudo apt full-upgrade && \
sudo apt install git \
zip \
unzip \
curl \
nginx-extras \
mariadb-server-10.5 \
php7.4-fpm \
php7.4-cli \
php7.4-gd \
php7.4-cgi \
php7.4-curl \
php7.4-imap \
php7.4-bcmath \
php7.4-intl \
php7.4-pspell \
php7.4-mysql \
php7.4-zip \
php7.4-xmlrpc \
php7.4-xml \
php7.4-sqlite3 \
php7.4-opcache \
php7.4-json \
php7.4-readline \
php7.4-soap \
php7.4-mbstring \
php7.4-ldap \
php7.4-phpdbg  \
php7.4-interbase \
libphp7.4-embed \
php7.4-odbc \
php7.4-tidy \
php7.4-sybase \
php7.4-bz2 \
php7.4-common \
php-tokenizer \
php-imagick \
php-pear \
php-xdebug
```

## Configurar MariaDB

```bash
sudo mysql_secure_installation
```

## Instalar Composer

```bash
php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" && \
sudo php composer-setup.php --install-dir=/usr/local/bin --filename=composer && \
php -r "unlink('composer-setup.php');"
```

## Instalar Node

```bash
curl -sL https://deb.nodesource.com/setup_14.x | sudo -E bash - && \
sudo apt install -y nodejs && \
sudo apt install -y build-essential
```

## Instalar Gulp CLI de forma global

```bash
sudo npm install gulp-cli -g
```

## Instalar WP-CLI

```bash
curl -O https://raw.githubusercontent.com/wp-cli/builds/gh-pages/phar/wp-cli.phar && \
chmod +x wp-cli.phar && \
sudo mv wp-cli.phar /usr/local/bin/wp
```

## Instalar PHP CodeSniffer

```bash
sudo pear channel-update pear.php.net && \
sudo pear install PHP_CodeSniffer
```

## Instalar WordPress Coding Standards + WPThemeReview Standard + PHPCompatibility + PHPCompatibilityWP + PHPCompatibilityParagonie

```bash
sudo git clone -b master https://github.com/WordPress-Coding-Standards/WordPress-Coding-Standards.git \
$(pear config-get php_dir)/PHP/CodeSniffer/src/Standards/WordPress && \
sudo git clone -b master https://github.com/WPTT/WPThemeReview.git \
$(pear config-get php_dir)/PHP/CodeSniffer/src/Standards/WPThemeReview && \
sudo git clone -b master https://github.com/PHPCompatibility/PHPCompatibility.git \
$(pear config-get php_dir)/PHP/CodeSniffer/src/Standards/PHPCompatibility && \
sudo git clone -b master https://github.com/PHPCompatibility/PHPCompatibilityWP.git \
$(pear config-get php_dir)/PHP/CodeSniffer/src/Standards/PHPCompatibilityWP && \
sudo git clone -b master https://github.com/PHPCompatibility/PHPCompatibilityParagonie.git \
$(pear config-get php_dir)/PHP/CodeSniffer/src/Standards/PHPCompatibilityParagonie
```

## Configurar PHP_CodeSniffer

```bash
sudo phpcs --config-set installed_paths $(pear config-get php_dir)/PHP/CodeSniffer/src/Standards/WordPress,$(pear config-get php_dir)/PHP/CodeSniffer/src/Standards/WPThemeReview,$(pear config-get php_dir)/PHP/CodeSniffer/src/Standards/PHPCompatibilityParagonie,$(pear config-get php_dir)/PHP/CodeSniffer/src/Standards/PHPCompatibility,$(pear config-get php_dir)/PHP/CodeSniffer/src/Standards/PHPCompatibilityWP
```

## Cambiar en nombre de usuario de php7.4-fpm y de nginx

Es para poder trabajar en la carpeta del usuario y así evitar problemas de permisos.

```bash
sudo sed -i 's/www-data/'"$USER"'/g' /etc/php/7.4/fpm/pool.d/www.conf && \
sudo sed -i 's/www-data/'"$USER"'/g' /etc/nginx/nginx.conf
```

## Crear un pequeño script para poder iniciar, reiniciar y apagar los servicios

Crear un archivo con el nombre que quieras (Ej: WebServer) en la ruta `/usr/local/bin/`

```bash
sudo touch /usr/local/bin/WebServer
```

Pagar estos comandos dentro del fechero `/usr/local/bin/WebServer`

```bash
#!/bin/bash
sudo service mariadb $1 && \
sudo service nginx $1 && \
sudo service php7.4-fpm $1
```

Dar el permiso de ejecución al archivo

```bash
sudo chmod +x /usr/local/bin/WebServer
```

Ahora podemos usar `WebServer start`, `WebServer restart`, `WebServer stop`
