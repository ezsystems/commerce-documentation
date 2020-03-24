# How to: Change button color (css)

Please make sure before you start to install the [boilerplate](Extending-the-frontend_23561043.html) first

## Introduction

In this example we want to give an easy example how to add your own css file or how to override the styling with plan css.

## Adding a custom.css to the frontend structure

Lets start by creating a new css file in the folder web/css. Name this file "custom.css" or else.

**Custom.css**

``` css
/* Our custom.css file */
.button {background-color: green;)
```

In the custom.css we have created add the content from the codeblock above.

In the next step we want to link the new css file inside the template (pagelayout.html.twig)

Go to the file `app/Resources/views/pagelayout.html.twig`

``` html+twig
{% block stylesheets %}
{% stylesheets
'/css/style.css'
'bundles/silversolutionseshop/css/ez-blocks.css'
'bundles/sisocontentplugin/css/component.content.css'
%}
<link rel="stylesheet" href="{{ asset_url }}"/>
<link rel="stylesheet" href="/css/custom.css"/>
{% endstylesheets %}
```

Please add the line `<link rel="stylesheet" href="/css/custom.css"/>` in the `<head>` section of the `pagelayout.html.twig`

Now the new `custom.css` file is overriding all the vendor css and you can start overriding the styles of the project.

!!! note

    If you don't see any changes please remove the cache!
