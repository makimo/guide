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

### Architectural Best-Practices

#### 0. Terminology

Selectors - pure functions whose purpose is to encapsulate query to the
database.

```python
def select_designation_count():
    return Members.objects.values('designation').annotate(count=Count('designation'))
```

Selector's name has to start with `select_`. Finally, selectors should live in
`<app>/selectors.py` file.

#### 1. Overview

In Django, business logic should live in:

1. Model properties
2. Pure-functions (e.g. selectors)
3. `clean` method for extra validation
4. Views (Controllers in MVC terminology)\*

On the other hand, business logic should not live in:

1. Templates, form tags
2. `save` method.
3. Forms/Serializers
4. API

In general, try to stick by the rule: fat models and pure-functions
and thin everything-else.

\*Views should be used only for calling and orchestrating other objects.
By themselves they should have very little state.

#### 2. Selectors vs. Properties vs. Managers vs. Inline

In general, simple queries should be written directly in the code.

```python
x = X.objects.filter(y=z)
```

Properties should be used when queries: will be reused, are non-trivial,
relate to a single model.

```python
class Product(models.Model):
    ...

    @property
    def lowest_price(self):
        try:
            return self.prices.all().order_by('-value').first()
        except ObjectDoesNotExist:
            return None
```

However, selectors should be used when queries:

1. Will be reused.
2. Are relatively complex.
3. Relate to multiple models/modules.

Therefore, in an average project, selectors should be used sparingly -
i.e. whenever we want to clearly separate some complex query for testability
and readability. Additionally, by putting selectors into separate file we
automatically flag them as important.

```python
def select_aggregate_set_items_by_type(set):
    '''
    This method calculates number of items in the Set by PartType.
    E.g.: "Brick Type A: 16, Leaflet Type A: 4"
    '''
    c = Counter(defaultdict(lambda: 0))

    for item in set.elements.all().distinct():
        if item.is_part:
            quantity = Membership.objects.get(
                item=item,
                container=set
            ).quantity

            c[mem.part.type.name] += quantity
        else:
            c += select_aggregate_set_items_by_type(mem.set)

    return dict(c)
```

Similar to selectors are managers. Managers are mostly used as groupings
of model-related queries which are not bound to a particular instanace
(i.e. they are static). In contrast, however, managers are by default
related to a single model - do not use them to perform queries on multiple
models/modules.

```python
class EntryManager(models.Manager):
    ...

    def select_todays_entries_starting_with_what_excluding_with_food(self):
        return self.get_queryset().filter(headline__startswith="What") \
            .filter(pub_date__lte=datetime.date.today()) \
            .exclude(body_text__icontains="food")
            .order_by('pub_date')
```

#### 3. Data-model Validation

When validation you want to perform relates to a data-model (as opposed to e.g.
serialiation rules or form-field format), write in the `clean` method of
the relevant model and call `full_clean` from `save`.
Raise `ValidationError` if data-model is validated. Remember to handle errors
in the view/form.

_Rationale_

Data-model constraints are expressed on the levels: database and application.
In Django application, database-level constraints are expressed declaratively
in models (e.g. when you write `ForeignKey` relation or specify `unique=True`).
On the other hand, application-level constraints have to  expressed imperatively
by the programmer in Python. For consistency and readability, it is
preferrable that those rules are:

1. Encapsulated in pure-functions, i.e. do not mutate the state.
2. Are stored closely to related models.
3. Are always upheld.

Therefore, `clean` is a good candidate.
