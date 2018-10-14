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

### URL Naming

Here are some tips for creating readable URL resource structure.

#### 1. Path Params vs. Query Strings

Use path params to locate the resource and query strings to parametrize
the query.

```python
def get(self, request, object_id):
    option = self.request.GET.get('option', 'default')
```

#### 2. Plural vs. Singular

Use plural form to indicate the fact that a resource is a list. Use
singular if object returned is a singleton.

```
/users
/users/{id}/config
```

#### 3. Nesting Resources

A good rule of thumb regarding resource resting is to nest resources if
a child candidate resources cannot exist outside of its parent. Otherwise
we may end up with multiple paths refering to the same resource.

```
/users/{user_id}/config
/user-configs?user_id={user_id}
/user-configs/{user_id}
/configs?type=user-configs&cfg_id={cfg_id}
```

This is very ugly.

Of course, it _may_ be more convienient and readable to break this rule.
In such case, ensure that nesting is done only on strong relations.

```
/author/{author_id}/books
/books/{book_id}
```

#### 4. Additional Reading

1. [REST API Tutorial - Resource Naming](
https://www.restapitutorial.com/lessons/restfulresourcenaming.html)

2. [REST URL Parameter Naming Conventions](
https://stackoverflow.com/questions/26357211/rest-url-path-parameter-naming-conventions)
