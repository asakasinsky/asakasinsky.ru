---
layout: post
title: "Виртуальное окружение Python на хостинге"
date: 2012-06-23 02:26
comments: true
categories: 
- узелки на память
tags:
- "разработка"
- Python
description: "Сегодня возьмём и установим необходимые вам python-пакеты на хостинг. Для этого мы развернем виртуальное рабочее окружение virtualenv."
---
Бы­ло у&nbsp;вас так, что на&nbsp;shared-хо­стин­ге не&nbsp;ока­за­лось необ­хо­ди­мых вам python-па­ке­тов? У&nbsp;ме­ня бы­ло. Так как я&nbsp;про­по­ве­дую прин­цип&nbsp;— «Ско­рее все­го эта про­бле­ма уже воз­ни­ка­ла у&nbsp;дру­гих лю­дей, и, ско­рее все­го есть ре­ше­ние», Гугл до­воль­но быст­ро вы­вел ме­ня на&nbsp;virtualenv. В&nbsp;мо­ем слу­чае все ока­за­лось до­ста­точ­но про­сто.

За­хо­дим по&nbsp;SSH на&nbsp;хо­стинг, ка­ча­ем све­жий ар­хив с&nbsp;virtualenv.
{% codeblock virtualenv lang:html http://pypi.python.org/pypi/virtualenv "источник" %}
wget http://pypi.python.org/packages/source/v/virtualenv/virtualenv-1.7.2.tar.gz
{% endcodeblock %} 
<!--more-->
Рас­па­ко­вы­ва­ем, за­хо­дим в&nbsp;по­лу­чен­ную пап­ку и&nbsp;со­зда­ем вир­ту­аль­ное окру­же­ние в&nbsp;пап­ке <code>~/myvenv</code>. В&nbsp;этой пап­ке, бу­дут на­хо­дит­ся уста­нов­лен­ные Python-па­ке­ты.
{% codeblock %}
tar xzf ~/virtualenv-1.7.2.tar.gz
cd ~/virtualenv-1.7.2
python virtualenv.py ~/myvenv
{% endcodeblock %} 

Теперь нужно активировать виртуальное окружение.
{% codeblock %}
source ~/myvenv/bin/activate
{% endcodeblock %}

Про­ве­рить ра­бо­ту мож­но с&nbsp;по­мо­щью ко­ман­ды <code>which python</code>, она долж­на вы­дать путь к&nbsp;Python на­хо­дя­ще­му­ся в&nbsp;ва­шем вир­ту­аль­ном окру­же­нии. Ес­ли нуж­но де­ак­ти­ви­ро­вать окру­же­ние, ис­поль­зо­вать Python, уста­нов­лен­ный в&nbsp;си­сте­ме, нуж­но вве­сти в&nbsp;кон­со­ли <code>deactivate</code>. При­ме­чу, что у&nbsp;ме­ня пе­ре­клю­че­ние ре­жи­мов вид­но в&nbsp;кон­со­ли сле­ва пе­ред име­нем те­ку­ще­го поль­зо­ва­те­ля. Мо­жет ока­зать­ся по­лез­ным вклю­че­ние ко­ман­ды ак­ти­ва­ции вир­ту­аль­но­го окру­же­ния в <code>~/.bashrc</code>.

С&nbsp;ма­ги­ей virtualenv мож­но озна­ко­мит­ся с&nbsp;по­мо­щью <code>man virtualenv</code>. На­при­мер:
{% codeblock "перевод некоторых строк из man virtualenv" %}
--no-site-packages
           Полностью вас изолирует от глобальных библиотек системы

--relocatable
           Делает виртуальное окружение относительным, то есть вы
           сможете перемещать ваше виртуальное окружение.

--clear
           Полностью вычищает виртуальное окружение от установленных
           пакетов и настроек.
{% endcodeblock %} 

Если заглянуть в папку <code>~/myvenv/bin</code>, то увидим следующее:
{% codeblock "структура ~/myvenv/bin" %}
bin
├── activate
├── easy_install
├── pip
├── python
└── ...
{% endcodeblock %} 
Зна­чит это, что у&nbsp;нас в&nbsp;рас­по­ря­же­нии изо­ли­ро­ван­ные ути­ли­ты easy_install и&nbsp;pip, с&nbsp;по­мо­щью ко­то­рой мож­но уста­нав­ли­вать нуж­ные па­ке­ты прак­ти­че­ски от­ку­да угод­но. Вот так:
{% codeblock %}
pip install some_package
# или
pip install -e svn+http://svn.somesite.com/trunk/

# ещё есть отличная возможность установки пакетов 
# в окружение, не активируя его.
pip install -E ~/myvenv/ some_package
{% endcodeblock %} 

Практический пример работы с pip:
{% codeblock %}
# установим django.
pip install django
# и, к примеру SQLAlchemy ORM
pip install sqlalchemy
# причём, прошу заметить можно устанавливать пакеты какой-то
# определенной версии

# совершенно полезнючая штука - формируем список установленных пакетов
pip freeze > packages.txt

# допустим мы передали этот список другому разработчику
# и он устанавливает эти пакеты у себя
pip install -r packages.txt
{% endcodeblock %} 

