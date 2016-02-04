---
layout: post
title:  "Fix Jekyll 3 jekyll-pagenate issue"
date:   2016-02-04 15:28:00 +1100
categories: jekyll
tags: jekyll
---

After Jekyll is upgraded to 3. Old Jekyll website may have the following issue when starting this website. 

       Deprecation: You appear to have pagination turned on, but you 
       haven't included the `jekyll-paginate` gem. Ensure you have 
        `gems: [jekyll-paginate]` in your configuration file.


To sovle this issue, just add this line to _config.yml file.

       gems: [jekyll-paginate]

If a new post is not generated, it may be a timezone issue.       

   