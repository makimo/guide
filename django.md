## Django-specific conventions

### Project bootstrapping

Use `django-template`.

```
cookiecutter git@bitbucket.org:makimo/django-template.git
```

### Templates

Nobody cares whether HTML in the browser is indented nicely or not. So the
focus shifts to make templates programmer-friendly, treating logical blocks 
and HTML elements as first-class citizens for legibility.

```
{% block content %}
<ul>
    {% for x in y %}
        <li>
            {% if x.photo %}
                <img src="{{ x.photo }}">
            {% endif %}

            <p>{{ x.text }}</p>
        </li>
    {% endfor %}
</ul>
{% endblock %}
```

Note: You can omit indentation after global scope `{% block %}` elements.
