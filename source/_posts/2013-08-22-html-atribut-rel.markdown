---
layout: post
title: "HTML - атрибут rel"
date: 2013-08-22 05:05
comments: true
categories:
- короткие заметки
- читай спецификацию
---
Внезапно открыл что атрибут тэга «a» — «rel» является [_space-separated set_](http://www.w3.org/TR/2012/WD-html5-20121025/links.html#attr-hyperlink-rel). А значит это, что мы вполне можем использовать несколько значений, разделенных пробелом.  
Пример:  
    <a href="/chapter/1" rel="chapter next prefetch">Глава первая</a>
