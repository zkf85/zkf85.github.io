---
layout: post
title: "Sort Tags Alphabetically"
date: "2016-11-30 20:32:00"
category: [Web Development]
tags: [jekyll, html, css, liquid, tag, sort]
---
<div class = "message">
Unordered tags make a Jekyll tag list look sloppy, so if you care about the details, you likely want your tag list to be alphabetized.
<br>KF 11/30/2016
</div>

## Sort Tags A ~ Z
### Reference
- [Michael Lanyon - Alphabetizing Jekyll Page Tags In Pure Liquid (Without Plugins)](https://blog.lanyonm.org/articles/2013/11/21/alphabetize-jekyll-page-tags-pure-liquid.html)
- [Advanced Liquid: Sort](https://www.siteleaf.com/blog/advanced-liquid-sort/)

Based on the notes from [Michael Lanyon](https://blog.lanyonm.org), I modified my `tags.html` to the version below:

<!--more-->

{% raw %}
```html
---
layout: page
title: Tags
---
<!-- tag sorted - kf 11/30/2016 -->

{% capture site_tags %}{% for tag in site.tags %}{{ tag | first }}{% unless forloop.last %},{% endunless %}{% endfor %}{% endcapture %}
<!-- site_tags: {{ site_tags }} -->
{% assign tag_words = site_tags | split:',' | sort %}
<!-- tag_words: {{ tag_words }} -->
<div class="tags-expo">
  <div class="tags-expo-list">
  {% for tag in tag_words %}
    <a href="#{{ tag | cgi_escape }}" class="post-tag">{{ tag }} <span style="font-size:80%; color:gray;">{{ site.tags[tag] | size }}</span></a>
  {% endfor %}
  </div>
  
  <div class="tags-expo-section">
    {% for item in (0..site.tags.size) %}{% unless forloop.last %}
    {% capture this_word %}{{ tag_words[item] | strip_newlines }}{% endcapture %}
  <h2 id="{{ this_word | cgi_escape }}">{{ this_word }}</h2>
    <ul class="tags-expo-posts">
      {% for post in site.tags[this_word] %}{% if post.title != null %}
        <a class="post-title" href="{{ site.baseurl }}{{ post.url }}">
        <li>
        {{ post.title }} &laquo 
      <small class="post-date">{{ post.date | date_to_string }}</small>
      </li>
      </a>
      {% endif %}{% endfor %}
    </ul>
    {% endunless %}{% endfor %}
  </div>
</div>
```
{% endraw %}

For `categories.html` the basic idea is the same.

{% raw %}
When Adding the code block above, I came across a curious problem when writing liquid template code in this markdown file. When Jekyll compiles the static web files, it tries to process all the curly brackets like: `{{` and `{%`.
{% endraw %}

This is not a bug , but a [caveat of using a templating language with markdown](https://github.com/jekyll/jekyll/issues/814). The workarounds are provided by 

- [Ozzie Liu - Writing Liquid Template in Markdown Code Blocks with Jekyll](http://ozzieliu.com/2016/04/26/writing-liquid-template-in-markdown-with-jekyll/).

**Simply put, just add`{% raw %}{% {% endraw %}raw %}` and `{% raw %}{% {% endraw %}endraw %}` on the both end when {% raw %}`{{` or `{%` appears.{% endraw %}**

> It is pretty confused to render the above line!!!


## Advanced - Sort Tags A~Z Case Insensitive
### Reference
- [Add case-insensitive sort #529](https://github.com/Shopify/liquid/issues/529)
[Sort only sorts in binary #2135](https://github.com/jekyll/jekyll/issues/2135)

### Solution is simple:
- `some_list | sort: 'downcase'` for strings


<br><br>
***KF***