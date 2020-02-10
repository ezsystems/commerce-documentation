#  Component - List Blog 

In addition to our [generic list component ](Component---List_23560980.html)we have prepared Blog specific styling.

## Sass

**File location**

``` 
scss/storm/_components.list-blog.scss
```

### Sass settings:

Use this variable inside project settings in order to override default settings.

``` 
$blog-highlight-color: $flat-peter-river;
```

### List of available classes

| Class                 | Description                                   |
| --------------------- | --------------------------------------------- |
| .c-list\_\_item--blog | Additional class that is applied to each item |

## HTML

**List item**

``` 
<div class="c-list__item c-list__item--blog">
    <h2 class="c-list__title">Lorem ipsum</h2>
    <img class="c-list__thumb" src="http://fillmurray.com/100/1100" alt="">
    <div class="c-list__short-description">
        <p>Lorem ipsum dolor sit amet, consectetur adipisicing elit. Provident at perferendis repellat, illum, voluptatum tenetur eos. Tempore ab perspiciatis, delectus harum recusandae dolorum vero sapiente nemo praesentium itaque. Tenetur, ab!</p>
    
    <div class="c-list__author">Frank Dege
    <div class="c-list__date">12.02.2016
    <a class="c-list__comments-link" href="#">
        Comments
    </a>
    <a class="c-list__more-link" href="#">read more</a>
```

## Twig

**src/Siso/ContentBundle/Resources/views/line/blog\_post.html.twig**

``` 
<div class="c-list__item c-list__item--blog">

  <div class="row">
    <div class="columns">
      <h2 class="u-no-margin-top">
        <a class="c-list__title" href="{{ path( 'ez_urlalias', {'locationId': content.contentInfo.mainLocationId} ) }}">
          {{ ez_render_field( content, 'title') }}
        </a>
      </h2>

  <div class="row">
    <div class="columns">

      {% if content.fields['image'] is defined %}
        {{ ez_render_field( content, 'image', {
          'attr': {
            'class': 'c-list__thumb left'
          },
          'parameters': {
            'alias': 'medium'
          } }) }}
      {% endif %}

      {% if content.fields['blog_post_intro'] is defined %}
        {{ ez_render_field( content, 'blog_post_intro', {
          'attr': {
            'class': 'c-list__short-description'
          }
        }) }}
      {% endif %}

  <div class="row">

    <div class="medium-6 columns js-meta">

      {% if content.fields['author'] is defined %}
        {{ ez_render_field( content, 'author', {
          'attr': {
            'class': 'c-list__author'
          }
        }) }}
      {% endif %}

      {% if content.fields['publication_date'] is defined %}
        {{ ez_render_field( content, 'publication_date', {
          'attr': {
            'class': 'c-list__date'
          }
        }) }}
      {% endif %}

      <a class="c-list__comments-link"
         href="{{ path( 'ez_urlalias', {'locationId': content.contentInfo.mainLocationId} ) }}#disqus_thread"
         data-disqus-identifier="content_{{ content.contentInfo.mainLocationId }}">
        {{ '... read more'|st_translate() }}
      </a>

    <div class="medium-6 columns small-text-right">

      <a class="button tiny c-list__more-link"
         href="{{ path( 'ez_urlalias', {'locationId': content.contentInfo.mainLocationId} ) }}">
        {{ '... read more'|st_translate() }}
      </a>

```
