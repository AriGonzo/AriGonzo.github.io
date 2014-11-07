---
layout: default
title: "!bang"
---
#{{page.title}}

###a blog about surviving The Iron Yard

###from the brain and fingers of [Ari Gonzalez](http://www.twitter.com/arigonzoari)

####[My GitHub Profile](http://www.github.com/AriGonzo)


## Who Am I?

A lifelong technophile and a product of the generation that straddles Wikipedia and Dewey Decimal. Netflix and Blockbuster Video. With a strong love and respect for technology and the internet, I'm taking the plunge and immersing myself in all things coding. Follow along as I chronicle my journey from coding novice to Jr Web Developer in Orlando's first Iron Yard Academy Front End Engineering cohort.  

## Blog Posts:
<ul class="posts">
    {% for post in site.posts %}
      <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ post.url }}">{{ post.title }}</a></li>
    {% endfor %}
  </ul>

## Repositories I Own:
* [This Blog](https://github.com/AriGonzo/AriGonzo.github.io)
* [FEE--2014--Fall](https://github.com/AriGonzo/FEE--2014--FALL)
* [Overachiever](https://github.com/AriGonzo/Overachiever)
* [TIY-Assignments](https://github.com/AriGonzo/TIY-Assignments)
