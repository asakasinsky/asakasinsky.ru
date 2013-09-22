---
layout: post
title: "grunt-recess ошибка Parser error"
date: 2013-05-19 00:13
comments: true
categories:
- короткие заметки
---

**Проблема**: grunt-recess споткнулся на строчке

```
filter:progid:DXImageTransform.Microsoft.dropshadow(OffX=0,OffY=1,Color=#ccffffff,Positive=true);
```

Ошибка *Parser error*. Путём проб выявил неприязнь линтера к использованию альфа-канала в HEX-формате цвета. Нашёл решение на <https://github.com/cloudhead/less.js/issues/734>


**Решение**:

Заставить линтер LESS пропустить значение *filter*.

```
filter:~"progid:DXImageTransform.Microsoft.dropshadow(OffX=0,OffY=1,Color=#ccffffff,Positive=true)";
```
