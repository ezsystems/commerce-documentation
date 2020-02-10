#  Component - List 

Generic list component that can be used for news, events, blog posts, etc.

In order to extend the list component for specific type of content please create a new file following this naming convention: \_components.list-TYPE.scss e.g: \_components.list-blog.scss.

Currently supported specific types:

  - [list blog](Component---List-Blog_23560977.html)
  - [list event](Component---List-Blog_23560977.html)

## Sass

**File location**

``` 
scss/storm/_components.card.scss
```

### Sass settings:

Use these variable inside project settings in order to override default settings.

``` 
// list item
$list-spacing: $column-gutter;
$list-border-width: rem-calc(1);
$list-border-color: $secondary-color;
$list-left-border-width: rem-calc(10);

// item elements
$list-title-font-size: $h2-font-size;
$list-title-border: rem-calc(1) solid $secondary-color;
$list-sub-title-font-size: $h3-font-size;
$list-date-font-size: rem-calc(14);
$list-comments-font-size: rem-calc(14);
$list-author-font-size: rem-calc(14);
$list-more-link-font-size: rem-calc(12);
$list-location-font-size: rem-calc(14);

// meta section (date, author, etc)
$list-meta-line-height: rem-calc(36); // same height like the tiny button (... read more)
$list-location-icon-color: $primary-color;
$list-date-icon-color: $primary-color;
```

In order to change settings in project find settings/\_storm.scss file in your project and find the List section.

### List of available classes

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th>Class</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>.c-list</td>
<td>Main wrapper for the whole list. No need to aplly to single element on detail page</td>
</tr>
<tr>
<td>.c-list__item</td>
<td>Single item wrapper</td>
</tr>
<tr>
<td>.c-list__item--full</td>
<td>Modified version of single wrapper selector. Removes margin and padding. Used on detail pages</td>
</tr>
<tr>
<td>.c-list__title</td>
<td>Item main title</td>
</tr>
<tr>
<td>.c-list__sub-title</td>
<td>Item subtitle</td>
</tr>
<tr>
<td><p>.c-list__description</p>
<p>.c-list__short-description</p></td>
<td>Short and long description. Since the content comes from eZ it's a wrapper for &lt;p&gt;. Check the code below for details</td>
</tr>
<tr>
<td>.c-list__thumb</td>
<td>Item image</td>
</tr>
<tr>
<td>.c-list__date</td>
<td>Date field</td>
</tr>
<tr>
<td>.c-list__author</td>
<td>Author field</td>
</tr>
<tr>
<td>.c-list__author-bio</td>
<td>Author bio wrapper. Used on detail pages</td>
</tr>
<tr>
<td>.c-list__comments-link</td>
<td>Comments link</td>
</tr>
<tr>
<td>.c-list__more-link</td>
<td>More link on a list</td>
</tr>
<tr>
<td>.c-list__embed-object</td>
<td>Emebed object eg. product inside a detail list item</td>
</tr>
</tbody>
</table>

## HTML

**List item**

``` 
<div class="c-list__item">
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

**Detail page**

``` 
<div class="row">
    <div class="large-9 columns">
        <div class="c-list__item c-list__item--full">
        
            <h1 class="c-list__title">Lorem ipsum</h1>
            <h2 class="c-list__sub-title">Lorem ipsum</h2>
            <div class="c-list__date">12.02.2016
            <img class="c-list__thumb" src="http://fillmurray.com/100/1100" alt="">
            <div class="c-list__short-description">
                <p>Lorem ipsum dolor sit amet, consectetur adipisicing elit. Provident at perferendis repellat, illum, voluptatum tenetur eos. Tempore ab perspiciatis, delectus harum recusandae dolorum vero sapiente nemo praesentium itaque. Tenetur, ab!</p>
            
            <div class="c-list__description">
                <p>Lorem ipsum dolor sit amet, consectetur adipisicing elit. Provident at perferendis repellat, illum, voluptatum tenetur eos. Tempore ab perspiciatis, delectus harum recusandae dolorum vero sapiente nemo praesentium itaque. Tenetur, ab!</p>

    <div class="large-4 columns">
        <div class="c-list__author-bio">
            <h3>Lorem ipsum</h3>
            <img src="author-img.jpg" alt="">
            <a href="mailto:name@domain.com">name@domain.com</a>

```

## Twig

**Twig - src/Siso/ContentBundle/Resources/views/line/blog\_post.html.twig**

``` 
{#
    Render a content object in line view
#}

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

**Twig - src/Siso/ContentBundle/Resources/views/full/blog\_post.html.twig**

``` 
{% extends "SilversolutionsEshopBundle::pagelayout.html.twig"|st_resolve_template %}
{% block breadcrumb %}{% endblock %}
{% block content %}

  <div class="row u-margin-top-4x c-list__item c-list__item--full">
    <div class="medium-9 columns">

      <h1 class="c-list__title u-no-border">{{ ez_render_field( content, 'title' ) }}</h1>

      <h2 class="c-list__sub-title">{{ ez_render_field( content, 'subline' ) }}</h2>

      <div class="u-margin-bottom-2x">
        <span class="c-list__date">
          {{ location.contentInfo.publishedDate|localizeddate( 'short', 'short', app.request.locale ) }}

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

      {% if content.fields['body'] is defined %}
        {{ ez_render_field( content, 'body', {
          'attr': {
            'class': 'c-list__description'
          }
        }) }}
      {% endif %}
      {% if not ez_is_field_empty( content, 'comments_allowed' ) %}
        <div class="u-margin-top-4x">
          <div id="disqus_thread">
          <script>
            /**
             * RECOMMENDED CONFIGURATION VARIABLES: EDIT AND UNCOMMENT THE SECTION BELOW TO INSERT DYNAMIC VALUES FROM YOUR PLATFORM OR CMS.
             * LEARN WHY DEFINING THESE VARIABLES IS IMPORTANT: https://disqus.com/admin/universalcode/#configuration-variables
             */
            /*
             var disqus_config = function () {
             this.page.url = PAGE_URL; // Replace PAGE_URL with your page's canonical URL variable
             this.page.identifier = PAGE_IDENTIFIER; // Replace PAGE_IDENTIFIER with your page's unique identifier variable
             };

             */
            var disqus_config = function () {
              this.page.url = '{{ path( 'ez_urlalias', {'locationId': content.contentInfo.mainLocationId} ) }}';
              this.page.identifier = 'content_{{ content.contentInfo.mainLocationId }}';
              this.page.title = '{{ ez_render_field( content, 'title' ) }}';
            };
            (function () { // DON'T EDIT BELOW THIS LINE
              var d = document, s = d.createElement('script');

              s.src = '//silvereshop.disqus.com/embed.js';

              s.setAttribute('data-timestamp', +new Date());
              (d.head || d.body).appendChild(s);
            })();
          </script>
          <noscript>Please enable JavaScript to view the <a href="https://disqus.com/?ref_noscript" rel="nofollow">comments
              powered by Disqus.</a></noscript>
        
      {% endif %}

    <div class="medium-3 columns">

      <div class="panel">
        <div class="c-list_author-bio">
          {% set authorId = ez_field_value( content, 'author' ) %}
          {{ render( controller( "ez_content:viewContent", {"contentId": authorId.destinationContentId, "viewType": "embed"} ) ) }}

{% endblock %}
```
