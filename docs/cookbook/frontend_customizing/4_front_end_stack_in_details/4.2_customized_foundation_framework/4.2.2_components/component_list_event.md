#  Component - List Event 

In addition to our [generic list component ](Component---List_23560980.html)we have prepared Event specific styling.

## Sass

**File location**

``` 
scss/storm/_components.list-event.scss
```

### Sass settings:

Use this variable inside project settings in order to override default settings.

``` 
$event-highlight-color: $flat-amethyst;
```

### List of available classes

| Class                  | Description                                   |
| ---------------------- | --------------------------------------------- |
| .c-list\_\_item--event | Additional class that is applied to each item |

## HTML

**List item**

``` 
<div class="c-list__item c-list__item--event">
    <h2 class="c-list__title">{{ ez_render_field( content, 'title') }}</h2>
    <div class="c-list__location">
        Paris, France
    
    <div>
        <div class="c-list__date">12.02.2016 - <div class="c-list__date">14.02.2016
    
    <div class="c-list__date">Publication date: 15.01.2016

```

## Twig

**src/Siso/ContentBundle/Resources/views/line/event.html.twig**

``` 
{#
    Render a content object in line view
#}

<div class="c-list__item c-list__item--event">
  <div class="row">
    <div class="columns">
      <h2 class="c-list__title">{{ ez_render_field( content, 'title') }}</h2>

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

  <div class="row">

    <div class="columns">

      {% if content.fields['location'] is defined %}
        {{ ez_render_field( content, 'location', {
          'attr': {
            'class': 'c-list__location'
          }
        }) }}
      {% endif %}

      {% if content.fields['from_time'] is defined %}
        {{ ez_render_field( content, 'from_time', {
          'attr': {
            'class': 'c-list__date u-no-margin-right'
          }
        }) }}

        {% if content.fields['to_time'] is defined %}
          - {{ ez_render_field( content, 'to_time', {
          'attr': {
            'class': 'c-list__date'
          }
        }) }}
        {% endif %}
      {% endif %}

      {% if content.fields['publication_date'] is defined %}
        {{ ez_render_field( content, 'publication_date', {
          'attr': {
            'class': 'c-list__date'
          }
        }) }}
      {% endif %}

```
