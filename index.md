---
layout: page
title: Welcome to Forbajato's Techblog!
tagline: Documenting tech challenges and wins
---
{% include JB/setup %}
    
# Recent Posts

<ul class="posts">
  {% for post in site.posts %}
    <h2> {{ post.title }} </h2>
    <p> {{ post.summary }} </p>
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ post.url }}">Read More</a></li>
    <br>
   
  {% endfor %}
</ul>

##To Do

* Add blog roll
* Add tag cloud
* Add about section
* More stuff in the TODO list.
* Subscribe <a href="/atom.xml">via Atom</a>.

