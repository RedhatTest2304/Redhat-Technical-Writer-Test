---
layout: default
---

{% for post in site.posts %}
  <article>
    {{ post.content }}
  </article>
{% endfor %}
