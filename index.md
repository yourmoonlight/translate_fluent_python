---
layout: homepage
title: 流利的Python
date: 2016-05-07
modifiedOn: 2016-05-07
---

<h2 id="preface">前言</h2>

- [版权申明](preface/license.html)
- [开场白](preface/intro.html)

<h2 id="prologue">序章 Python 数据模型</h2>

- [Python 数据模型](prologue/the_python_data_model.html)

<h2 id="data structures">数据结构</h2>

<h2 id="functions as objects">“一级公民”函数</h2>

<h2 id="object oriented idioms">面向对象的一些行话，风格</h2>

<h2 id="control flow">非一般的流程控制</h2>

<h2 id="metaprogramming">黑魔法元编程</h2>

<h2 id="appendix">后记</h2>

---

{% comment %}

{% if site.posts.size != 0 %}

## 最新文章

{% for post in site.posts %}
* {{ post.date | date_to_string }} [{{ post.title }}]({{ post.url }})
{% endfor %}

{% endif %}

{% if site.pages.size != 0 %}

## 最新页面

{% for page in site.pages limit:5 %}
{% if page.url !='/index.html' %}
* [{{ page.title }}]( {{ page.url }})（{{ page.date }}）
{% endif %}
{% endfor %}

{% endif %}

{% endcomment %}
