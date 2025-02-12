---
layout: default
---

## Connect with me
- [Download my resume](./assets/resume.pdf)
- [Email](mailto:msg@nakul.net)
- [Linkedin](https://www.linkedin.com/in/thenakulchawla/)
- [Stack Overflow](https://stackoverflow.com/users/4057016/thenakulchawla)

## Blog Posts

{% for post in site.posts %}
- [{{ post.title }}]({{ post.url }})
{% endfor %}
