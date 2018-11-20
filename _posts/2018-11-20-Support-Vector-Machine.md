---
layout: post
title: Support Vector Regression
category: example
comments: true
description: ..
tags:
    - SVR
    - machine learning
---
이 포스트에서는 Support Vector Regression(SVR) 의 개념을 이해하고자 합니다. 또한, 간단한 형태의 데이터를 이용하여 SVR 의 요소들이 어떠한 의미를 가지고 있는지 눈으로 확인해 보도록 하겠습니다.

아래 내용은 고려대학교 강필성 교수님의 Business-Analytics 강의와
Support Vector Regression 논문(Basak, D., Pal, S., & Patranabis, D. C. , 2007),
고려대학교 DMQA 연구실 박사과정 이민정님의 코드를 참고하여 작성되었습니다.
잘못된 부분이 있으면 연락 바랍니다.

## Sample Data

𝒚=𝟑+𝒍𝒐𝒈(𝒙)+𝒔𝒊𝒏(𝒙) 의 식에 정규분포를 따르는 노이즈를 주어 50개의 샘플 데이터를 생성하였습니다. 이 샘플 데이터를 이용하여 SVR 이 어떤 원리로 작동되는지 알아보도록 하겠습니다.

<figure>
<img alt="Sample Data" src="/resources/images/sample_data_01.jpg"/>
<figcaption>
<strong>Figure 1: </strong> Sample Data
</figcaption>
</figure>



>The first rule of any technology used in a business is that automation applied to an efficient operation will magnify the efficiency. The second is that automation applied to an inefficient operation will magnify the inefficiency.
><footer><cite> - Bill Gates</cite></footer>
{: .blockquote cite="http://www.worldwildlife.org/who/index.html" }

Class aptent taciti sociosqu ad litora torquent per conubia nostra, per inceptos himenaeos.
Nunc orci purus, egestas a erat pulvinar, fringilla sodales turpis. Fusce luctus eu eros at maximus.

## This is h2 header

Class aptent taciti sociosqu ad litora torquent per conubia nostra, per inceptos himenaeos.
Nunc orci purus, egestas a erat pulvinar, fringilla sodales turpis. Fusce luctus eu eros at maximus.


<a name="definition-dfa"></a>
{: .bottomless }

{::options parse_block_html="true" /}

<div class="env-header">
Definition {{ site.definitions[0].number }}
</div>

<div class="definition alert">
{{ site.definitions[0].content }}
</div>

{::options parse_block_html="false" /}


Class aptent taciti sociosqu ad litora torquent per conubia nostra, per inceptos himenaeos.
Nunc orci purus, egestas a erat pulvinar, fringilla sodales turpis. Fusce luctus eu eros at maximus.

{::options parse_block_html="true" /}

<div class="info alert">
**INFO:** This is info message. `This is code`. [This is link](#). _This is italic_.
</div>

Class aptent taciti sociosquad litora torquent per conubia nostra, per inceptos himenaeos.

<div class="warning alert">
**WARNING:** This is warning message. `This is code`. [This is link](#). _This is italic_.
</div>

Class aptent taciti sociosqu ad litora torquent per conubia nostra, per inceptos himenaeos.

<div class="note alert">
**NOTE:** This is note message. `This is code`. [Definition 1.0](#definition-dfa). _This is italic_.
</div>

<div class="success alert">
#### This is success message! ####

`This is code`. [This is link](#). _This is italic_.

Mauris sed augue molestie, git clone ac consequat erat posuere. Interdum et malesuada fames ac ante ipsum primis in faucibus. Curabitur odio sapien, sollicitudin ut pharetra in, tincidunt vel elit.

* List item number 1
    * Nested list item number 1
    * Curabitur pharetra eget ante at vestibulum. Mauris ullamcorper lacus sed augue molestie, ac consequat erat posuere.
* List item number 2
* List item number 3

Curabitur pharetra eget ante at vestibulum.  
</div>

{::options parse_block_html="false" /}

### This is h3 header

Curabitur pharetra eget ante at vestibulum. Mauris ullamcorper lacus sed augue molestie, ac consequat erat posuere. Interdum et malesuada fames ac ante ipsum primis in faucibus. Curabitur odio sapien, sollicitudin ut pharetra in, tincidunt vel elit.

<div class="env-header">HTML/CSS</div>
{% highlight html linenos %}
<!DOCTYPE html>
<html>
<head>
   <style>
      p {
          border-style: solid;
          border-bottom: thick dotted #ff0000;
        }
   </style>
</head>
<body>
    <p>This is some text in a paragraph.</p>
</body>
</html>
{% endhighlight %}


Class aptent taciti `sociosqu ad litora torquent` per conubia nostra, per inceptos himenaeos.
Nunc orci purus, egestas a erat pulvinar, fringilla sodales turpis. `Fusce luctus` eu eros at maximus.

- List item number 1
    - Nested list item 1
    - Nested list item 2
        - Deep nested list item 1ante at vestibulum. Mauris ullamcorper lacus sed augue molestie.
        - Deep nested list item 2
    - Nested list item number 3
- List item number 3

1. List item number 1
    1. Nested list item number 1
    2. Curabitur pharetra eget ante at vestibulum. Mauris ullamcorper lacus sed augue molestie, ac consequat erat posuere.
2. List item number 2
3. List item number 3


#### This is h4 header

Curabitur pharet<sub>r</sub>a eget ante at vestibulum. Mauris ullamcorper lacus sed augue molestie, ac consequat erat posuere. Interdum et malesuada fames ac ante ipsum primis in faucibus. Curabitur odio sapien, sollicitudin ut pharetra in, tincidunt vel elit.

{::options parse_block_html="true" /}

<figure class="table">

| Header One     | Header Two | Header Three | Head |
| ------------- | ------------- | ----------- | -------- |
| Item One       | Item Two       | Item Three   | Long Item Four   |
| Item One       | Item Two       | Item Three   | Long Item Four   |
| Item One       | Item Two       | Item Three   | Long Item Four   |
| Item One       | Item Two       | Item Three   | Long Item Four   |
| Item One       | Item Two       | Item Three   | Long Item Four   |
| Item One       | Item Two       | Item Three   | Long Item Four   |
| Item One       | Item Two       | Item Three   | Long Item Four   |

<figcaption>
**Table 1:** Eestie, ac consequat erat posuere.
</figcaption>
</figure>

{::options parse_block_html="false" /}

Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nunc rhoncus luctus quam in gravida.
Mauris rutrum ullamcorper pellentesque. Aliquam posuere euismod erat, ac consequat leo ullamcorper non.

ac consequat erat posuere. Interdum et malesuada fames ac ante ipsum primis in faucibus. Curabitur odio sapien, sollicitudin ut pharetra in, tincidunt vel elit.

<figure>
<img alt="image test" src="/resources/images/main_front.jpg"/>
<figcaption>
<strong>Figure 1: </strong>Curabitur pharetra eget ante at vestibulum. Mauris ullamcorper lacus sed augue molestie, ac consequat erat posuere.
</figcaption>
</figure>

Bold text: __I am bold__

Italic text: *I am italic*

Inline code: `html`, `css`, `git clone <url>`.

Link text: [more examples](http://www.dennis-grinch.co.uk)

Abbreviation: <abbr title='Content Management System'>CMS</abbr>
