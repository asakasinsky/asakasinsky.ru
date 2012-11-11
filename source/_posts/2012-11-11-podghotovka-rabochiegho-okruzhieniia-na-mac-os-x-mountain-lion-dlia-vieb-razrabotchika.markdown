---
layout: post
title: "Подготовка рабочего окружения на Mac OS X Mountain Lion для веб разработчика"
date: 2012-11-11 01:36
comments: true
categories: 
- узелки на память
tags: 
- "разработка"
- "Best practice"
description: "Подготовка рабочего окружения на Mac OS X Mountain Lion для веб разработчика"
---
{% img center http://screenshots.en.sftcdn.net/en/scrn/3340000/3340291/mountain-lion-skin-pack-01-100x100.png 'Mac OS X Mountain Lion' 'Mac OS X Mountain Lion' %}


> Эта статья что-то вроде памятки, потому многое не развернуто. Если возникнут вопросы, задавайте в комментариях, отвечу.

<!--more-->
## Приложения


- Chrome
- Firefox
- Opera
- iTerm
- FileZilla
- ImageOptim


## Редактор


{% codeblock «Sublime Text» — делаем запуск из терминала. lang:bash %}
# Нам нужен будет доступ к этой папке.   
ssudo chown -R `whoami` /usr/local
# Проверяем работу CLI  
open /Applications/Sublime\ Text\ 2.app/Contents/SharedSupport/bin/subl
# Если открылся Sublime Text, значит все ОК. Создадим симлинк на subl:  
sudo ln -s /Applications/Sublime\ Text\ 2.app/Contents/SharedSupport/bin/subl /usr/local/bin/sublime  
# Мягко примем изменения:  
touch ~/.bashrc  
open ~/.bashrc  
# Вносим  в файл  
# --------------------------------------  
export PATH=/usr/local/bin:$PATH  
# --------------------------------------  
# Примем изменения, не выходя из системы  
source ~/.bashrc  
sudo chmod 0777 /usr/local/bin
# Тестируем  
sublime имя-файла-или-каталога
{% endcodeblock %} 

## Переключаемся на Zshell

{% codeblock Zshell lang:bash %}
curl -L https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh | sh  
touch ~/.zshrc  
sublime ~/.zshrc   
# Вносим в файл
# --------------------------------------  
ZSH=$HOME/.oh-my-zsh  
ZSH_THEME="kphoen"  
plugins=(git osx rails ruby github node npm brew)  
source $ZSH/oh-my-zsh.sh  
export&nbsp;PATH=/usr/local/bin:/usr/local/sbin:/usr/bin:/bin:/usr/sbin:/sbin:/usr/local/mysql/bin:/usr/X11/bin  
# --------------------------------------
{% endcodeblock %} 



## Устанавливаем Xcode Command Line Tools
Xcode > Preferences > Downloads > Command Line Tools

{% codeblock Command Line Tools lang:bash %}
# Проверяем:   
gcc --version
# Если видим 686-apple-darwin…., то все ОК
{% endcodeblock %} 

## Ставим X11

[Качаем](http://xquartz.macosforge.org/trac/wiki) инсталлятор и ставим.  
{% codeblock X11 lang:bash %}
# Ставим симлинк:  
sudo ln -s /opt/X11 /usr/X11
{% endcodeblock %} 

## Ставим Homebrew — пакетный менеджер

<code>
ruby <(curl -fsSk https://raw.github.com/mxcl/homebrew/go)
</code>  
Следуем инструкциям  

<code>
brew doctor
</code>  
Если получаем __Your system is raring to brew__, то все ОК.

## Ставим Git

{% codeblock Git lang:bash %}
brew install git
# Проверяем:
which git

# Конфиг:  
touch ~/.gitconfig  
sublime ~/.gitconfig  
# Вносим в файл
# --------------------------------------  
[user]  
	name = Имя, на латинице  
	email = email
[github]  
	user = ваше имя на github  
[core]  
	editor = sublime -w  
[color]  
	ui = true  
# --------------------------------------
{% endcodeblock %} 


## Ставим RVM для Ruby 1.9.3

{% codeblock RVM lang:bash %}
curl -L https://get.rvm.io | bash -s stable \-\-ruby</code>  
# Вбиваем:
type rvm | head -1

# Если не видим rvm is a function, то
echo '[[ -s "$HOME/.rvm/scripts/rvm" ]] && . "$HOME/.rvm/scripts/rvm" # Load RVM function' >> ~/.zlogin
source ~/.zlogin 

#Проверяем: 
type rvm | head -1

{% endcodeblock %} 


## Ставим Ruby 1.9.3

{% codeblock Ruby lang:bash %}
# Обновляем RVM до последней версии:
rvm get head

#Подготовка и установка:
rvm requirements
brew update
brew install libksba
brew tap homebrew/dupes
brew install autoconf automake apple-gcc42
rvm pkg install openssl
rvm install 1.9.3 # или rvm reinstall all --force если у вас уже установлен Ruby)

sudo ln -s /usr/local/bin/gcc-4.2 /usr/bin/gcc-4.2 
# Говорим, чтобы использовалась именно эта версия:
rvm use 1.9.3 --default

#Проверяем:
ruby -v

{% endcodeblock %} 

## Сразу поставим Octopress

{% codeblock Octopress lang:bash %}
cd путь/к/директории-куда-хотим-поставить-octopress
git clone git://github.com/imathis/octopress.git octopress
rvm --create use 1.9.3@octopress
rvm use 1.9.3@octopress
cd octopress
# На вопрос доверять ли файлу .rvmrc говорим 'y' или 'yes'
gem install bundler
bundle install
# Устанавливаем текущую тему Octopress
rake install 
{% endcodeblock %} 

## Ставим нужные пакеты

<code>brew install mc git ack wget curl memcached libmemcached readline sqlite gdbm pkg-config</code>

## Ставим Python

{% codeblock Python lang:bash %}
brew install python --framework --universal
sublime ~/.zshrc # Вносим в файл
# --------------------------------------
  export PATH=/usr/local/share/python:$PATH
    if [ -d "$HOME/bin" ] ; then
      export PATH="$HOME/bin:$PATH"
    fi
# --------------------------------------
# Меняем симлинки
cd /System/Library/Frameworks/Python.framework/Versions
sudo rm Current
sudo ln -s /usr/local/Cellar/python/2.7.3/Frameworks/Python.framework/Versions/Current
# Ставим pip и virtualenv
easy_install pip
pip install virtualenv
pip install virtualenvwrapper
source /usr/local/share/python/virtualenvwrapper.sh
{% endcodeblock %} 

## Ставим MongoDB

{% codeblock MongoDB lang:bash %}
brew install mongodb
mkdir -p ~/Library/LaunchAgents
ln -s /usr/local/opt/mongodb/*.plist ~/Library/LaunchAgents/ 
launchctl load -w ~/Library/LaunchAgents/homebrew.mxcl.mongodb.plist 

# Проверка:
mongo
{% endcodeblock %} 

## Ставим MySQL

<code>brew install mysql</code>
Далее следуем инструкции

## Ставим PHP

Несмотря на наличие интерпретатора PHP в «коробочной поставке» OSX, будем собирать версию PHP из исходников. 
Причины:
* Последняя версия
* 64-бит
* [mysqlnd](http://habrahabr.ru/post/154663/)
* Нативная поддержка Юникода
* Механизм мониторинга процесса загрузки файла на сервер
* Опыт

{% codeblock PHP 5.4 lang:bash %}
# Необходимые пакеты:  
brew install gmp gettext icu4c libmcrypt libjpeg libxml2

cd ~/Downloads/
wget http://ru2.php.net/get/php-5.4.6.tar.bz2/from/ru2.php.net/mirror
mv mirror php-5.4.6.tar.bz2
bunzip2 php-5.4.6.tar.bz2 && tar xvf php-5.4.6.tar
cd php-5.4.6

env EXTRA_LIBS="-lstdc++" ./configure \
--prefix=/usr/local/php \
--with-apxs2=/usr/sbin/apxs \
--with-layout=GNU \
--enable-calendar \
--enable-ftp \
--enable-bcmath \
--enable-mbstring \
--with-icu-dir=/usr/local/Cellar/icu4c/49.1.2 \
--enable-intl \
--enable-sockets \
--enable-soap \
--with-gettext=/usr/local/Cellar/gettext/0.18.1.1 \
--with-bz2 \
--with-zlib \
--enable-zip \
--with-gd \
--with-jpeg-dir \
--with-png-dir=/usr/X11 \
--with-xpm-dir \
--with-freetype-dir=/usr/X11 \
--enable-exif \
--with-gmp \
--with-mcrypt \
--with-mysql=mysqlnd \
--with-mysqli=mysqlnd \
--with-pdo-mysql=mysqlnd \
--with-mysql-sock=/usr/local/var/mysql/mysqld.sock \
--with-curl \
--with-openssl \
--with-libxml-dir=/usr/local/Cellar/libxml2/2.8.0 \
--with-xsl=/usr \
--with-xmlrpc \
--enable-shmop \
--enable-pcntl \
--enable-sysvmsg \
--enable-sysvsem \
--enable-sysvshm

make && sudo make install

# Будет предложено сделать тесты, можно согласится

# Переименовываем старую версию
sudo mv /usr/bin/php /usr/bin/osx-php
sudo mv /usr/bin/phar /usr/bin/osx-phar
sudo mv /usr/bin/php-config /usr/bin/osx-php-config
sudo mv /usr/bin/phpize /usr/bin/osx-phpize

# Для удобства создаем символьные ссылки в /usr/local/bin.

ln -s /usr/local/php/bin/php /usr/local/bin/php ln -s /usr/local/php/bin/pear /usr/local/bin/pear ln -s /usr/local/php/bin/pecl /usr/local/bin/pecl ln -s /usr/local/php/bin/phar.phar /usr/local/bin/phar ln -s /usr/local/php/bin/php-config /usr/local/bin/php-config ln -s /usr/local/php/bin/phpize /usr/local/bin/phpize

# Редактируем php.ini
# По этой ссылке 
# https://gist.github.com/4052783
# Находится мой php.ini
sublime /etc/php.ini
{% endcodeblock %} 

## Настраиваем Apache2

{% codeblock Apache2 lang:bash %}
# Настраиваем виртуальные хосты
# Сайтам на локальной машине будет отведена доменная зона .dev
# Чуть позже настроим dns сервер, обслуживающий эту доменную зону
# Сайты будут располагаться в папке ~/Workspace
# Document Root будет в папке ~/Workspace/САЙТ/htdocs
# В папку сайта будут складываться логи
# Образец моего файла находится тут
# https://gist.github.com/4052809
touch ~/Workspace/httpd-vhosts.conf
sublime ~/Workspace/httpd-vhosts.conf
sudo ln -s ~/Workspace/httpd-vhosts.conf /etc/apache2/other

# Чуть-чуть меняем настройки Apache
# Образец моего файла находится тут
# https://gist.github.com/4052795
sublime "/etc/apache2/httpd.conf"

# Тестируем конфигурацию
sudo apachectl configtest
sudo apachectl restart # или sudo apachectl graceful
{% endcodeblock %} 

## Настраиваем SSH

{% codeblock SSH lang:bash %}
# Генерируем rsa-ключи. Полученные публичный и приватный ключи 
# будут лежать в пап­ке ~/.ssh
ssh-keygen -t rsa -C "мой-email@mail-bla-bla.com"

# До­ба­вим rsa-ключ аген­ту аутен­ти­фи­ка­ции ssh-agent.
ssh-add ~/.ssh/id_rsa

# Правим права
chmod 700 ~/.ssh
chmod 600 ~/.ssh/id_rsa
chmod 644 ~/.ssh/id_rsa.pub 
chmod 644 ~/.ssh/authorized_keys
chmod 644 ~/.ssh/known_hosts

# Выводим на консоль публичный ключ
# В настройках GitHub добавляем его содержимое
cat ~/.ssh/id_rsa.pub

# Тестируем соединение
# GitHub должен поприветствовать нас, если все получилось
ssh -T git@github.com

# Для удобства добавления ключей на сервера 
# существует утилита ssh-copy-id, но в OSX она 
# отсутствует, ставим ее
curl https://raw.github.com/beautifulcode/ssh-copy-id-for-OSX/master/ssh-copy-id.sh -o /usr/local/bin/ssh-copy-id
chmod +x /usr/local/bin/ssh-copy-id

# Теперь можно с легкостью добавлять публичный ключ на сервера (потребуется ввод пароля)
ssh-copy-id user@server # user - имя пользователя (логин), 
                        # и server - либо IP сервера, либо доменное имя 

# Входим по SSH (ура, без пароля!!!). 
ssh user@server         # user - имя пользователя (логин), 
                        # и server - либо IP сервера, либо доменное имя 

{% endcodeblock %} 

## Настраиваем функцию отправки почты

{% codeblock Mail lang:bash %}
sudo mkdir /Library/Server /Library/Server/Mail /Library/Server/Mail/Data /Library/Server/Mail/Data/spool /Library/Server/Mail/Data/spool/maildrop
sudo chown postfix:postdrop /Library/Server/Mail/Data/spool/maildrop
sudo chmod 777 /Library/Server/Mail/Data/spool/maildrop

# Создадим Simple Authentication and Security Layer (SASL) файл.
sudo nano /etc/postfix/sasl_passwd
# Вносим в файл, подставляя свои данные
# --------------------------------------
smtp.gmail.com:587 your.name@gmail.com:your.password
# --------------------------------------

# Сообщаем Postfix, где искать SASL.
sudo postmap /etc/postfix/sasl_passwd

# Конфигурируем службу Postfix
sudo nano /etc/postfix/main.cf
# Просто добавляем в конец файла эти строки
# --------------------------------------
# Minimum Postfix-specific configurations. mydomain_fallback = localhost mail_owner = postfix setgid_group = postdrop relayhost=smtp.gmail.com:587

# Enable SASL authentication in the Postfix SMTP client. smtp_sasl_auth_enable=yes smtp_sasl_password_maps=hash:/etc/postfix/sasl_passwd smtp_sasl_security_options=

# Enable Transport Layer Security (TLS), i.e. SSL. smtp_use_tls=yes smtp_tls_security_level=encrypt tls_random_source=dev:/dev/urandom
# --------------------------------------

# Запускаем службу Postfix
sudo postfix start

# Если ловим ошибку, то исправляем ее в main.cf (в сообщении будет видно, в какой строке ошибка)
# И перезагружаем  службу
sudo postfix reload

# Тестируем
date | mail -s test your.name@gmail.com
{% endcodeblock %} 

## Установка и настройка DNS сервера dnsmasq

{% codeblock dnsmasq lang:bash %}
brew install dnsmasq

mkdir /usr/local/etc 
cp $(brew --prefix dnsmasq)/dnsmasq.conf.example /usr/local/etc/dnsmasq.conf

# Настраиваем на обслуживание доменной зоны .dev
nano /usr/local/etc/dnsmasq.conf
# Просто добавляем в конец файла эти строки
# --------------------------------------
address=/dev/127.0.0.1
listen-address=127.0.0.1
# --------------------------------------

# Обеспечиваем автозапуск
sudo cp $(brew --prefix dnsmasq)/homebrew.mxcl.dnsmasq.plist /Library/LaunchDaemons
# Загружаем демон
sudo launchctl load -w /Library/LaunchDaemons/homebrew.mxcl.dnsmasq.plist
# Затачиваем систему
sudo -s
sudo mkdir -p /etc/resolver
sudo echo 'nameserver 127.0.0.1' > /etc/resolver/dev
# Очищаем DNS кэш
dscacheutil -flushcache
# Смотрим вывод следующей команды, в резолверах должен появится и наш, dev
scutil --dns
{% endcodeblock %} 

## Ставим Nodejs и grunt с плагинами

[grunt.js](http://habrahabr.ru/post/148274/)

{% codeblock Nodejs и grunt lang:bash %}
brew install node 
git clone http://github.com/isaacs/npm.git
cd npm
sudo make install

sublime ~/.zshrc 
# Вносим в файл
# --------------------------------------
export PATH=/usr/local/lib/node:/usr/local/share/npm/bin:$PATH
# --------------------------------------

source ~/.zshrc

npm install -g grunt npm install grunt-recess

# Или просто копируем заготовку
{% endcodeblock %} 

## Ставим оптимизатор изображений Imgo и плагин для grunt

{% codeblock Imgo и плагин для grunt lang:bash %}
brew install exiftool imagemagick optipng libjpeg gifsicle
brew install "https://raw.github.com/imgo/imgo-tools/master/Formula/pngout.rb"
brew install "https://raw.github.com/imgo/imgo-tools/master/Formula/defluff.rb"
brew install "https://raw.github.com/imgo/imgo-tools/master/Formula/cryopng.rb"
brew install "https://raw.github.com/imgo/imgo-tools/master/Formula/imgo.rb"
npm install -g grunt-imgo
{% endcodeblock %} 