---
layout: post
title: Enhancing Security in Active Directory; A Comprehensive Guide to Protected Users Group (Arabic)
date: 2024-08-06 21:01:00
description: Three paths to attack protected user and mitigation strategies
tags: Active-Directory
categories: Active-Directory
thumbnail: assets/img/13.jpg
---



#### ما هو قروب المستخدمين المحميين؟ 

 قروب المستخدمين المحميين هي ميزة أمان مقدمة في ويندوز سيرفر 2012 والإصدارات الأحدث من الاكتف دايركتوري.تم تصميم هذا القروب لتعزيز أمان الحسابات ذات الصلاحيات العالية من خلال تطبيق سياسات أمنية صارمة تخفف من أنواع معينة من هجمات سرقة كلمات المرور.
 تهدف هذه السياسات إلى الحد من أساليب وبروتوكولات المصادقة التي يمكن لأعضاء هذا القروب استخدامها، ومن ثم تقلل من مخاطر كشف كلمات المرور وإساءة استخدامها.


##### :المميزات 
<br/><br/>

<div dir="rtl">

يتم تقيد اعضاء قروب المستخدمين المحميين لاستخدام معايير التشفير المتقدمة (AES).
</div>

<br/><br/>

<div dir="rtl">


 لايتم استعمال NTLM authentication: يتم تعطيل NTLM authentication لأعضاء قروب المستخدمين المحميين يعد NTLM بروتوكول
 أقل أمانا وأكثر عرضة لهجمات سرقة كلمات المرور مثل pass-the-hash . 

</div>
<br/><br/>

<div dir="rtl">


تم تقليل عمر تذكرة Kerberos (TGT): للمستخدمين المحميين لتكون على مدة 4 ساعات بشكل افتراضي.
 وهذا يقلل من الفترة التي يمكن من خلالها إساءة استخدام التذكرة المسروقة.

</div>

<br/><br/>


<div dir="rtl">

لايتم تخزين كلمات المرور (no cached credentials): لا يتم تخزين كلمات المرور وبهاذا الحل يتم منع المهاجمين
 من الحصول على كلمات المرور المخزنه في Lsass.

</div>

<br/><br/>


<div dir="rtl">

تم تقيد تسجيل الدخول التفاعلي (Interactive Logon Restrictions): لا يمكن للأعضاء تسجيل الدخول بشكل تفاعلي الى اجهزة
الكمبيوتر التي لا تدعم سياسات الأمان التي تفرضها قروب المستخدمين المحميين.وهذا يضمن استخدام هذه الحسابات عالية القيمة فقط على أنظمة امنة وموثوقة.


</div>

<br/><br/>


#### لماذا نستخدم قروب المستخدمين المحميين؟ 

السبب الرئيسي لاستخدام قروب \"المستخدمين المحميين\" هو توفير أمان محسّن للحسابات ذات الصلاحيات العالية, غالبًا ما يتم استهداف هذه الحسابات من قبل المهاجمين بسبب صلاحياتهم العالية وامكانية وصول الحسابات لبيانات حساسة.





#### :الهجمات المحتملة 

على الرغم من التدابير الأمنية المحسنة التي يوفرها قروب \"المستخدمون المحميون\"، قد يستمر المهاجمون في محاولة استهداف هذه الحسابات. نحتاج نفهم هاذه الهجمات المحتملة وكيفية التخفيف منها 
<br/><br/>

<div dir="rtl">


لنفترض أن لدينا حساب admin2 ويملك صلاحية domain admin, سنقوم بمحاولة تخمين كلمة المرور password spray للحساب ثم الوصول للجهاز واستخراج كلمات المرور dump lssas, لنجد hash الخاص بحساب admin2

</div>

<br/><br/>

<div class="row" style="text-align: center;">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" max-width="855px" max-heigth="708px" path="assets/img/1.JPG" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
</div>

<div class="row" style="text-align: center;">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" max-width="455px" max-heigth="408px" path="assets/img/2.JPG" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
</div>

<br/><br/>


<div dir="rtl">
الان سيتم اضافة المستخدم (Admin2) إلى قروب المستخدمين المحميين ومن ثم Password Spray و Pass-the-hash 

</div>

<br/><br/>


<div class="row" style="text-align: center;">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" max-width="455px" max-heigth="408px" path="assets/img/3.JPG" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
</div>



<div class="row" style="text-align: center;">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" max-width="755px" max-heigth="608px" path="assets/img/4.JPG" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
</div>


<div class="row" style="text-align: center;">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" max-width="755px" max-heigth="608px" path="assets/img/5.JPG" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
</div>

هاذه المرة يتم الرد علينا برسالة مختلفة عن السابق, بعد اضافة المستخدم في قروب المستخدمين المحميين
<br/>
<div dir="rtl">
وأخيرًا، سيتم استخراج كلمات المرور dump hash وتبين أنه لم يتم تخزين الهاش الخاص بالمستخدم


</div>
<br/>
<div class="row" style="text-align: center;">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" max-width="455px" max-heigth="408px" path="assets/img/3-1.JPG" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
</div>

<br/><br/>

<div dir="rtl">
<br/>
 
<h5>سيناريو الهجوم الأول:</h5>


<br/>
يمكننا استهداف (admin2) المضاف في قروب المستخدمين المحميين إذا تمكنا من crack ntlm hash

</div>


<div dir="rtl">
سيتم استعمال Rubeus لإنشاء تذكرة AES256 باستخدام كلمة مرور التي تم الحصول عليها مسابقا

</div>

<br/><br/>

<div class="row" style="text-align: center;">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" max-width="455px" max-heigth="408px" path="assets/img/6.JPG" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
</div>

<div dir="rtl">

 باستخدام Rubeus يمكننا طلب تذكرة TGT بنجاح
</div>


<div class="row" style="text-align: center;">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" max-width="455px" max-heigth="408px" path="assets/img/7.JPG" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
</div>

<br/><br/>


<div dir="rtl">
 <br/>
 
  <h5> سيناريو الهجوم الثاني: </h5>
 
<br/>
 
 إذا تمكنا من الوصول إلى جهاز تم تسجيل دخول المستخدم admin2 فيه مسبقا, فيمكننا الحصول على تذكرة TGT


</div>

<br/><br/>

<div class="row" style="text-align: center;">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" max-width="455px" max-heigth="408px" path="assets/img/8.JPG" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
</div>

من ثم، يمكننا استيرادها بنجاح. 


<div class="row" style="text-align: center;">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" max-width="455px" max-heigth="408px" path="assets/img/9.JPG" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
</div>

<br/><br/>

<div dir="rtl">
<br/>




<h5>سيناريو الهجوم الثالث: </h5>
 
 
<br/>
يمكننا استهداف مستخدم محدد من قروب المستخدمين المحميين بهجوم password spray

</div>


<div dir="rtl">
 باستعمال الاداة Netexec مع تحديد Kerberos--
 


</div>

<br/><br/>



<div class="row" style="text-align: center;">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" max-width="555px" max-heigth="455px" path="assets/img/10.JPG" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
</div>



#### :كيفية التصدي

استخدام كلمة مرور معقدة: عادةً ما تجمع كلمة المرور القوية بين الأحرف الكبيرة والصغيرة والأرقام والعلامات الخاصة، مما يجعل من

 الصعب على المهاجمين تخمينها أو اختراقها. تجنب استخدام معلومات يمكن تخمينها بسهولة مثل أعياد الميلاد أو الكلمات الشائعة
 
 إن تحديث كلمات المرور بانتظام واستخدام كلمات مرور فريدة لحسابات مختلفة يعزز من أمانك.

المراقبة والتدقيق المنتظم: مراقبة وتدقيق أنشطة الأعضاء في قروب المستخدمين المحميين بشكل مستمر. ابحث عن عمليات تسجيل الدخول
 
 المشبوهة والأنشطة غير العادية التي قد تشير إلى وجود اختراق.

استخدام الأنظمة الآمنة: تأكد من أن أعضاء قروب المستخدمين المحميين يسجلون الدخول فقط إلى الأنظمة التي تحتوي على التحديثات

الأمنية وتدعم تدابير الأمان اللازمة.
<br/><br/>
<div dir="rtl">
اضافة طبقات أمان إضافية: استخدام تدابير أمان إضافية مثل MFA, PAM و Network segmentation.
</div>


تثقيف وتدريب المستخدمين: تثقيف وتدريب المستخدمين بانتظام على أفضل ممارسات الأمان وأهمية حماية كلمات المرور الخاصة بهم




#### خاتمة

يعد قروب المستخدمين المحميين في الاكتف دايركتوري اضافة قوية لتعزيز أمان الحسابات ذات الصلاحيات العالية  

من خلال فرض سياسات أمان صارمة والتخفيف من هجمات سرقة كلمات المرور. من خلال فهم ماهو هذه القروب

ولماذا يجب استخدامة، ومتى يتم استخدامه، والهجمات المحتملة ضده، يمكن للمؤسسات حماية حساباتها الأكثر أهمية

 بشكل فعال وضمان مستوى أعلى من الأمان عبر بيئة الاكتف دايركتوري الخاصة بها.تعد المراقبة المنتظمة
 
 وتحديثات النظام وتدابير الأمان الإضافية من المكونات الرئيسية في الحفاظ على سلامة قروب المستخدمين المحميين.
<br/><br/>

#### :لفهم قروب المستخدمين المحميين وتنفيذها بشكل أفضل، راجع المصادر التالية

[Microsoft Protected Users Security Group](https://learn.microsoft.com/en-us/windows-server/security/credentials-protection-and-management/protected-users-security-group)

[Microsoft Guidance about how to configure protected accounts](https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/manage/how-to-configure-protected-accounts)

