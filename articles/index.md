---
title: Listing All Articles
layout: default
---

<div>
  <h2 align="center">{{ page.title }}</h2>
</div>

<ul>
　　　　{% for post in site.posts %} 
　　　　　　<li>{{ post.date | date_to_string }} <a href="{{ site.baseurl }}{{ post.url }}">{{ post.title }}</a></li>
　　　　{% endfor %}
</ul>