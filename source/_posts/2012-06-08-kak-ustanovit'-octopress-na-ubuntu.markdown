---
layout: post
title: "Как установить Octopress на Ubuntu"
date: 2012-06-11 18:01
comments: true
categories: блоговодство
tags: Octopress 
description: "В статье рассказывается о том, как установить и настроить Octopress на Ubuntu. "
---
Итак, се­го­дня моя речь пой­дет о&nbsp;том, как по­ста­вить [Octopress](http://octopress.org/) на&nbsp;Ubuntu.
> Оговорюсь, что посмотреть актуальную версию Ruby можно посмотреть тут, [в документации.](http://octopress.org/docs/setup/) 
На&nbsp;мо­ем но­ут­бу­ке уста­нов­ле­на Ubnutu&nbsp;10.04, но&nbsp;ре­цепт бу­дет ра­бо­тать и&nbsp;на&nbsp;12.04. Сна­ча­ла под­го­то­вим­ся к&nbsp;уста­нов­ке Ruby&nbsp;1.9.2, так как имен­но эта вер­сия на&nbsp;дан­ный мо­мент необ­хо­ди­ма Octopress.
{% codeblock %}
aptitude install curl bison build-essential zlib1g zlib1g-dev openssl libssl-dev libreadline5 libreadline5-dev libreadline6 libreadline6-dev libxml2-dev git-core libyaml-dev libsqlite3-0 libsqlite3-dev sqlite3 libxml2-dev libxslt-dev autoconf libc6-dev ncurses-dev automake
{% endcodeblock %}
<!--more-->
{% pullquote %}
Для уста­нов­ки Ruby и со­зда­ния необ­хо­ди­мо­го на­бо­ра gem’ов (вот сло­веч­ко-то узнал, по­ка раз­би­рал­ся: {"gem /dʒem/&nbsp;— дра­го­це́нный ка́мень."}) по­ста­вим RVM - ме­не­джер вер­сий Ruby. Он за­од­но и за­ви­си­мо­сти дол­жен под­тя­нуть. В об­щем вся ру­ти­на на нем. Де­ла­ем все в тер­ми­на­ле. Кста­ти со­зда­те­ли Octopress'а по­зи­ци­о­ни­ру­ют свой фрейм­ворк, как *"A blogging framework for hackers."*. Все из-за ис­поль­зо­ва­ния тер­ми­на­ла, в ко­то­ром и блог уста­но­вишь и ста­тью на­бро­са­ешь и на сер­вер опуб­ли­ку­ешь.
{% endpullquote %}
{% codeblock %}
bash -s stable < <(curl -s https://raw.github.com/wayneeseguin/rvm/master/binscripts/rvm-installer)
{% endcodeblock %}

Да­лее мы&nbsp;про­сто до­бав­ля­ем две строч­ки в&nbsp;файл ва­ше­го про­фи­ля в&nbsp;bash. Это нуж­но для то­го, чтобы Ruby за­пус­кал­ся ав­то­ма­ти­че­ски каж­дый раз, ко­гда от­кры­ва­ем bash. С&nbsp;по­мо­щью ко­ман­ды source мы&nbsp;за­ста­вим си­сте­мы при­нять из­ме­не­ния немед­лен­но. 
{% codeblock %}
echo '[[ -s "$HOME/.rvm/scripts/rvm" ]] && . "$HOME/.rvm/scripts/rvm" # Load RVM function' >> ~/.bashrc
source .bashrc
{% endcodeblock %}

Пе­ре­за­пус­ка­ем кон­соль.  
На­би­ра­ем.  
<code> type rvm | head -n1 </code>  
В&nbsp;от­вет получаем.  
<code>rvm яв­ля­ет­ся функ­ци­ей</code>  
Ес­ли это так, то&nbsp;зна­чит уста­нов­ка RVM со­сто­я­лась успеш­но. Го­то­вим окру­же­ние и&nbsp;ста­вим Ruby.
{% codeblock %}
rvm pkg install openssl
rvm pkg install iconv
rvm install 1.9.2 -C --with-openssl-dir=$HOME/.rvm/usr,--with-iconv-dir=$HOME/.rvm/usr
rvm use 1.9.2 --default
rvm reload
rvm rubygems latest
gem install bundler
{% endcodeblock %}
{% pullquote %}
Кло­ни­ру­ем ре­по­зи­то­рий [Octopress](https://github.com/imathis/octopress). Тут нуж­но по­яс­нить, что кло­ни­ру­ем мы&nbsp;его в&nbsp;пап­ку octopress. В&nbsp;ней и&nbsp;бу­дет про­ис­хо­дить вся ма­гия ва­ше­го бло­га. {"Пап­ку мо­же­те на­звать как угод­но"}, ра­бо­тать с&nbsp;ней вам, но&nbsp;в&nbsp;этом при­ме­ре мы&nbsp;на­зо­вем её&nbsp;имен­но так&nbsp;— «octopress».
{% endpullquote %}
{% codeblock %}
git clone git://github.com/imathis/octopress.git octopress
{% endcodeblock %}

Переходим в эту папку.
{% codeblock %}
cd octopress/
{% endcodeblock %}

И... Оп-п-па, по­явит­ся за­прос от&nbsp;RVM. Ска­жи­те ему&nbsp;да, то&nbsp;есть yes.
{% codeblock %}
==============================================================================
= NOTICE                                                                     =
==============================================================================
= RVM has encountered a new or modified .rvmrc file in the current directory =
= This is a shell script and therefore may contain any shell commands.       =
=                                                                            =
= Examine the contents of this file carefully to be sure the contents are    =
= safe before trusting it! ( Choose v[iew] below to view the contents )      =
==============================================================================
Do you wish to trust this .rvmrc file? (/home/asakasinsky/octopress/.rvmrc)
y[es], n[o], v[iew], c[ancel]> yes
{% endcodeblock %}

Ставим Octopress.
{% codeblock %}
bundle install
bundle update
rake install
{% endcodeblock %}

Все! Octopress уста­нов­лен! Для ав­то­ма­ти­че­ско­го де­п­лой­мен­та уста­но­вим rsync.
{% codeblock %}
sudo apt-get install rsync
{% endcodeblock %}

Да­лее [на­стра­и­ва­ем SSH](/2012/06/12/skaz-o-ssh/) и&nbsp;кон­фи­гу­ри­ру­ем наш блог, точ­нее в&nbsp;фай­ле Rakefile в&nbsp;пап­ке на­ше­го бло­га из­ме­ня­ем сле­ду­ю­щие зна­че­ния в&nbsp;со­от­вет­ствии с&nbsp;на­строй­ка­ми SSH.
{% codeblock %}
ssh_user       = "user@domain.com"
document_root  = "~/public_html/"
rsync_delete   = true
deploy_default = "rsync"
{% endcodeblock %}

Все важ­ные на­строй­ки на­хо­дят­ся в&nbsp;од­ном фай­ле <code>_config.yml</code>. Обя­за­тель­но от­ре­дак­ти­руй­те его, там мно­го на­стро­ек по&nbsp;умол­ча­нию.

Краткий набор команд:
{% codeblock %}
rake generate    # генерирует статический сайт

rake preview     # можно запустить после генерации, 
                 # чтобы посмотреть на локальной машине.
                 # Сайт будет доступен по адресу: localhost:4000
                 # просто вбейте его в адресную строку браузера.

rake deploy      # Отсылает файлы сайта на сервер с помощью rsync

rake new_post["Name of your post"] # создает новый пост
rake new_page["Name of your page"] # создает новую страницу
{% endcodeblock %}
