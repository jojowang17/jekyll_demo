---
title: Categories
layout: default
---

<div id='tag_cloud'>
<p><font size = "4">Listing all categories:</font></p>
{% for cat in site.categories %}
<a href="#{{ cat[0] }}" title="{{ cat[0] }}" rel="{{ cat[1].size }}"><font color = "red" size = "4" face = "verdana">{{ cat[0] }} ({{ cat[1].size }})|</font></a>
{% endfor %}
</div>

<ul class="listing">
{% for cat in site.categories %}
  <li class="listing-seperator" id="{{ cat[0] }}"><font color = "red" size = "4" face = "verdana">{{ cat[0] }}</font></li>
{% for post in cat[1] %}
  <li class="listing-item">
  <time datetime="{{ post.date | "%Y-%m-%d" }}">{{ post.date | date:"%Y-%m-%d" }}</time>
  <a href="{{ site.url }}{{ post.url }}" title="{{ post.title }}">{{ post.title }}</a>
  </li>
{% endfor %}
</li>
{% endfor %}
</ul>

<script src="/media/js/jquery.tagcloud.js" type="text/javascript" charset="utf-8"></script> 
<script language="javascript">
$.fn.tagcloud.defaults = {
    size: {start: 1, end: 1, unit: 'em'},
      color: {start: '#f8e0e6', end: '#ff3333'}
};

$(function () {
    $('#tag_cloud a').tagcloud();
});
</script>
