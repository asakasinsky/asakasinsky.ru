---
layout: post
title: "HMVC и Sparks заработали вместе!!!"
date: 2012-06-24 00:50
comments: true
categories: 
- codeigniter
tags:
- "разработка"
description: "Ура! HMVC и Sparks заработали вместе. Вроде все работает. Update #1: Ан нет, не все."
---
Ура! HMVC и&nbsp;Sparks за­ра­бо­та­ли вме­сте. [Офи­ци­аль­ное ре­ше­ние про­бле­мы](http://getsparks.org/set-up-mx) у&nbsp;ме­ня так и&nbsp;не&nbsp;при­жи­лось, а&nbsp;от­вет на­шел­ся&nbsp;на [stack**overflow**](http://stackoverflow.com/questions/10214785/codeigniter-2-1-sparks-hmvclatest-cannot-load-libraries-sparks). Вро­де все ра­бо­та­ет.

**Update #1:**
Ан&nbsp;нет, не&nbsp;все. <!--more-->При уста­нов­ке ion_auth в&nbsp;ви­де спар­ка, в&nbsp;HMVC мо­ду­ле вы­лез­ла ошиб­ка о&nbsp;недо­ступ­но­сти язы­ко­во­го фай­ла. При­чем при ис­поль­зо­ва­нии обыч­но­го MVC кон­трол­ле­ра все ра­бо­та­ет. Ре­ше­ние на­шел на&nbsp;stack**overflow**:  

В&nbsp;фай­ле <code>sparks/ion_auth/2.2.4/libraries/Ion_auth.php</code> про­из­во­дим та­кую за­ме­ну.  
{% codeblock %}
-$this-&gt;lang-&gt;load('ion_auth', 'english');
+$this-&gt;lang-&gt;load('ion_auth', 'english', FALSE, TRUE, 'sparks/ion_auth/2.2.4/'); 
{% endcodeblock %}
По­сле это­го вы­лез­ла еще од­на ошиб­ка, гла­ся­щая, что недо­ступ­на ion_auth_model. Ошиб­ка то­же воз­ни­ка­ет при ис­поль­зо­ва­нии HMVC. Ре­шил гряз­ным, но&nbsp;ра­бо­чим ха­ком&nbsp;— пе­ред объ­яв­ле­ни­ем клас­са под­клю­ча­ем фай­лы:
{% codeblock %}
require_once BASEPATH.'core/Model.php';
require_once FCPATH.'sparks/ion_auth/2.2.4/models/ion_auth_model.php';
{% endcodeblock %}
В&nbsp;кон­тек­сте клас­са объ­яв­ля­ем свой­ство $ion_auth_model, ко­то­рое на­сле­ду­ем от&nbsp;клас­са Ion_auth_model.

Чи­тал слу­хи о&nbsp;том, что Sparks и&nbsp;HMVC со­би­ра­ют­ся сде­лать ча­стью яд­ра. Бы­ло&nbsp;бы кле­во, а&nbsp;то&nbsp;я&nbsp;по­смат­ри­ваю в&nbsp;сто­ро­ну Yii или Laravel. 

При­во­жу патч пол­но­стью.
{% codeblock %}
/**
 * file: sparks/ion_auth/2.2.4/libraries/Ion_auth.php
 */

@@ -19,6 +19,8 @@
 *
 */
 
+require_once BASEPATH.'core/Model.php';
+require_once FCPATH.'sparks/ion_auth/2.2.4/models/ion_auth_model.php';
 class Ion_auth
 {
 	/**
@@ -42,6 +44,8 @@
 	 **/
 	public $_extra_set = array();
 
+	public $Ion_auth_model = '';
+
 	/**
 	 * __construct
 	 *
@@ -53,8 +57,9 @@
 		$this->load->config('ion_auth', TRUE);
 		$this->load->library('email');
 		$this->load->library('session');
-		$this->lang->load('ion_auth');
-		$this->load->model('ion_auth_model');
+		$this->lang->load('ion_auth', 'english', FALSE, TRUE, 'sparks/ion_auth/2.2.4/'); 
+		//$this->load->model('ion_auth_model');
+		$this->ion_auth_model = new Ion_auth_model();
 		$this->load->helper('cookie');
 
 		//auto-login the user if they are remembered
{% endcodeblock %}

