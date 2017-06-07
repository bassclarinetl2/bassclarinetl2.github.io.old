---
layout: page
title: Welcome
tagline: 
---
{% include JB/setup %}

#I'm Will Heid and this is my job.
<img src="assets/img/pulling_fibre.png">
<p> Welcome. This site will contain my guides for Home Assistant and anyother musings or guides as circumstances need.


<ul class="posts">
  {% for post in site.posts %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>

