# Search templates

The following templates are used for search:

```
Search/search_results.html.twig
Search/search_facets.html.twig
Search/search_query.html.twig
Search/search_groups.html.twig
Search/search.html.twig
Search/search_filters.html.twig
Search/result/search_content_list/folder.html.twig
Search/result/search_content_list/video.html.twig
Search/result/search_content_list/event.html.twig
Search/result/search_content_list/search_content_footer.html.twig
Search/result/search_content_list/landing_page.html.twig
Search/result/search_content_list/article.html.twig
Search/result/search_content_list/file.html.twig
Search/result/search_content_list/blog_post.html.twig
```

## Line templates for content and products

Each Content Type requires a line template for search.
Line templates are responsible for displaying one product or one Content item. 

### Products

`Catalog/listProductNode.html.twig` is used for displaying the content for a product.
The template has access to the `catalogElement` containing all the data including price information. 

### Content items

Each Content Type requires a template. It is stored in `Search/result/search_content_list`.

In addition, the template has to be defined in the templating configuration:

``` yaml
search_content_list:
    siso_search_folder_item:
        template: SilversolutionsEshopBundle:Search/result/search_content_list:folder.html.twig
        match:
            Identifier\ContentType: [folder]
    siso_search_article_item:
        template: SilversolutionsEshopBundle:Search/result/search_content_list:article.html.twig
        match:
            Identifier\ContentType: [article]
    siso_search_landing_page_item:
        template: SilversolutionsEshopBundle:Search/result/search_content_list:landing_page.html.twig
        match:
            Identifier\ContentType: [landing_page]
    siso_search_blog_post_item:
         template: SilversolutionsEshopBundle:Search/result/search_content_list:blog_post.html.twig
         match:
             Identifier\ContentType: [blog_post]
    siso_search_event_item:
         template: SilversolutionsEshopBundle:Search/result/search_content_list:event.html.twig
         match:
             Identifier\ContentType: [event]
    siso_search_file_item:
         template: SilversolutionsEshopBundle:Search/result/search_content_list:file.html.twig
         match:
             Identifier\ContentType: [file]
    siso_search_video_item:
         template: SilversolutionsEshopBundle:Search/result/search_content_list:video.html.twig
         match:
             Identifier\ContentType: [video]
```

The template itself has access to the Content item.

Example for the blog post template (`Search/result/search_content_list/blog_post.html.twig`):

``` html+twig
{% block content %}
  {% if showImage and not ez_is_field_empty( content, 'image' ) %}
    <figure class="left c-card__image--inline u-no-float-on-small">
      {{ ez_render_field(content, 'image') }}
      {% if not ez_is_field_empty( content, 'caption' ) %}
        <figcaption>
          {{ ez_render_field( content, 'caption' ) }}
        </figcaption>
      {% endif %}
    </figure>
  {% endif %}
  <h3 class="c-card__title">
    <a href="{{ path(location) }}" title="{{ ez_field_value( content, 'title' ) }}">
      {{ ez_field_value( content, 'title' ) }}
    </a>
  </h3>
  {% if not ez_is_field_empty( content, 'blog_post_intro' ) %}
    <p class="c-card__description u-no-padding-top">
      {{ ez_render_field( content, 'blog_post_intro' )|truncate(maxchars) }}
    </p>
  {% endif %}
  {%  include 'SilversolutionsEshopBundle:Search/result/search_content_list:search_content_footer.html.twig'|st_resolve_template
     with {'category': 'Blog post'|st_translate('search'), 'contentDate': ez_field_value(content, 'publication_date')} %}
{% endblock %}
```

## Hiding the more / download link

In order to hide or remove the link make sure to pass `false` to the URI parameter.
This way you can, for example, hide the download button for a video, but keep it for all other Content Types.
You need to make some changes to a specific Content Type template:

``` html+twig
{% block content %}

  {% set fileInfo = ez_field_value(content,'file') %}
  {% set mimeType = fileInfo.mimeType |replace({'application/': ''})  %}

  {% set mimeText = '<span class="label info">' ~ mimeType ~ '' %}

  {% set uri = 'content/download/' ~ content.contentInfo.id ~ '/' ~
  content.contentInfo.currentVersionNo  ~
  '/file/' ~ fileInfo.fileName|escape( 'url' ) %}

  {% set uri = 'content/download/' ~ content.contentInfo.id ~ '/' ~ content.getField('file').id
  ~ '/version/' ~ content.contentInfo.currentVersionNo ~  "/file/"
  ~ fileInfo.fileName|escape( 'url' ) %}

  <h3 class="c-card__title">
    <a href="{{ path(location) }}" title="{{ ez_field_value( content, 'name' ) }}">
      {{ ez_field_value( content, 'name' ) }}
    </a>
  </h3>

  <div class="flex-video">
    <video controls>
      <source src="{{ uri }}" type="{{ mimeType }}">
      {{ 'common.video_not_supported'|st_translate }}
    </video>

  {% include 'SilversolutionsEshopBundle:Search/result/search_content_list:search_content_footer.html.twig'|st_resolve_template
  with {
        'category'  : 'Video'|st_translate('search'), 'contentDate': '',
        'buttonName': 'common.download_video'|st_translate,
        'addInfos'  : ' ' ~ mimeText ~ ', Size'|st_translate('search') ~ ': ' ~ (fileInfo.fileSize/1024)|number_format ~ ' KB',
        'uri'       : false
   } %}

{% endblock %}
```

When including `search_content_footer.html.twig`, make sure to pass `false` to the URI parameter.
