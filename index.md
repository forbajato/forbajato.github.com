---
layout: page
title: Welcome to Forbajato's Techblog!
tagline: Documenting tech challenges and wins
---
{% include JB/setup %}
    
## Recent Posts

<ul class="posts">
  {% for post in site.posts %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>

##To Do

Update the way the post list looks, improve summary, etc.
