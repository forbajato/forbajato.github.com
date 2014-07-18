---
layout: page
title: Welcome to Forbajato's Techblog!
tagline: Documenting tech challenges and wins
---
{% include JB/setup %}
    
# How do you get a page to build on github?

This page builds fine using Jekyll locally, git hub doesn't give any errors.  I wonder what it takes to get it to build on github?

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
* Subscribe <a href="/feed.xml">via RSS</a>.

