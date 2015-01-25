---
layout: post
title:  "Add tags to Jekyll static website"
date:   2015-01-25 20:42:28+1000
categories: jekyll
tags: jekyll
---
We can put tags in posts, e.g. adding "tags: jekyll" in the header of each post. There are two ways to show tags on a static Jekyll website without any plugin.

1. To put all tags in a html file, create a link on the main page to link this tag page. This page can be created based on
   the reference.

   http://stackoverflow.com/questions/1408824/an-easy-way-to-support-tags-in-a-jekyll-blog

2. Creating sperate tag pages includes three steps. The first two steps can follow the reference. The third step is as the below codes in "_layouts/post.html".

         <h3>Tags:
         {% raw  %}{% for tag in page.tags %}{% endraw %}
         <a class="page-link" href="/tags/{{tag}}">{{ tag }}</a>&nbsp;
         {% raw  %}{% endfor %}{% endraw %}

   Reference:http://christianspecht.de/2014/10/25/separate-pages-per-tag-category-with-jekyll-without-plugins/    
