#  4.6.2 Twig / HTML 

**Inspired by:**

  - <http://codeguide.co/>
  - <https://google.github.io/styleguide/htmlcssguide.xml>

Every line of code should appear to be written by a single person, no matter the number of contributors.

# Twig

## Documentation

Each Twig template could provide a doc block header. 

``` 
{#
/**
* @file Renders embed template for file content.
*
* @usedby used eZ Flow (or a controller)
* @param content 
*
* @module content
*/
#}
```

## Naming variables

Use camel case naming convention

``` 
// correct
{%set elementCounter %}
 
// NOT correct
{%set ElementCounter %}
{%set element_counter %}
```

## Folder structure

1.  Try to use Symfony standard when possible 
2.  Use Components for any other type of content  
      
3.  .....

## Use absolute to link assets in emails

In order to make sure all the assets like images inside emails are loaded from the correct url use absolute settings.

``` 
// Correct
{{ asset('bundles/sheracommon/img/email-header.png', absolute=true) }}
 
// NOT correct
{{ app.request.uriForPath(asset('bundles/sheracommon/img/email-header.png')) }} 
```

# HTML

## Syntax

  - Use soft tabs with two (2) spaces - they're the only way to guarantee code renders the same in any environment.

  - Nested elements should be indented once (two spaces).

  - Always use double quotes, never single quotes, on attributes.

  - Don't include a trailing slash in self-closing elements—the [HTML5 spec](http://dev.w3.org/html5/spec-author-view/syntax.html#syntax-start-tag) says they're optional.

    ``` 
    // recommended 
    <br>
    <hr>
     
    // not recommended
    <br />
    <hr />
    ```

  - Don't forget to close opened tags (\</li\>, \</body\>, etc)

## HTML5 doctype

``` 
<!DOCTYPE html>
```

## Language attribute

From the HTML5 spec:

> Authors are encouraged to specify a lang attribute on the root html element, giving the document's language. This aids speech synthesis tools to determine what pronunciations to use, translation tools to determine what rules to use, and so forth.

``` 
<html lang="en-us">
  <!-- ... -->
</html>
```

## IE compatibility mode

<span class="status-macro aui-lozenge aui-lozenge-error">READ MORE ABOUT IT

Internet Explorer supports the use of a document compatibility `<meta>` tag to specify what version of IE the page should be rendered as. Unless circumstances require otherwise, it's most useful to instruct IE to use the latest supported mode with **edge mode**.

For more information, [read this awesome Stack Overflow article](http://stackoverflow.com/questions/6771258/whats-the-difference-if-meta-http-equiv-x-ua-compatible-content-ie-edge-e). 

``` 
<meta http-equiv="X-UA-Compatible" content="IE=Edge">
```

## Character encoding 

``` 
<head>
  <meta charset="UTF-8">
</head>
```

## CSS and JavaScript includes

Per HTML5 spec, typically there is no need to specify a`type` when including CSS and JavaScript files as `text/css`and `text/javascript` are their respective defaults.

``` 
// recommended
<link rel="stylesheet" href="code-guide.css">
<script scr="script.js">

// NOT recommended
<link rel="stylesheet" href="code-guide.css" type="style/css">
<script type="text/javascript" scr="script.js">
```

Attribute order

 HTML attributes should come in this particular order for easier reading of code.

  - `class`
  - `id`, `name`
  - `data-*`
  - `src`, `for`, `type`, `href`, `value`
  - `title`, `alt`
  - `role`, `aria-*`

Classes make for great reusable components, so they come first. Ids are more specific and should be used sparingly (e.g., for in-page bookmarks), so they come second.

``` 
<a class="..." id="..." data-toggle="modal" href="#">
  Example link
</a>

<input class="form-control" type="text">

<img src="..." alt="...">
```

## Boolean attributes

  - no need to declare a value for a boolean attribute

``` 
// recommended
<input type="text" disabled>
<input type="text" required>
<input type="checkbox" value="1" checked>
<select>
  <option value="1" selected>1</option>
</select>
 
// NOT recommended
<input type="text" disabled="disabled">
<input type="text" required="required">
<input type="checkbox" value="1" checked="checked">
<select>
  <option value="1" selected="selected">1</option>
</select>
```

## Reducing markup

Whenever possible, avoid superfluous parent elements when writing HTML. Many times this requires iteration and refactoring, but produces less HTML. Take the following example:

``` 
<!-- Not so great -->
<span class="avatar">
  <img src="...">
<!-- Better -->
<img class="avatar" src="...">
```

## Omit the protocol from embedded resources.

``` 
<!-- Recommended -->
<script src="//www.google.com/js/gweb/analytics/autotrack.js"></script>
 
<!-- Not recommended -->
<script src="http://www.google.com/js/gweb/analytics/autotrack.js"></script>
 
// CSS
/* Recommended */
.example {
  background: url(//www.google.com/images/example);
}
 
/* Not recommended */
.example {
  background: url(http://www.google.com/images/example);
}
```

## Use only lowercase.

All code has to be lowercase: This applies to HTML element names, attributes, attribute values (unless `text/CDATA`), CSS selectors, properties, and property values (with the exception of strings).

``` 
<!-- Not recommended -->
<A HREF="/">Home</A>

<!-- Recommended -->
<img src="google.png" alt="Google">

/* Not recommended */
color: #E5E5E5;

/* Recommended */
color: #e5e5e5;
```

Remove trailing white spaces.

 Trailing white spaces are unnecessary and can complicate diffs. Space is visualised with an underscore

``` 
<p>Text</p>_ <-- the underscore shows the space which is not good practice
```

Use buttons instead of input filed with a submit type

 With buttons you have more flexibility (you can use icons with text, etc). Additionally the text inside a button will break if there is no space left.

``` 
// correct
<button type="submit">Submit</button>
 
// NOT correct
<input type="submit" value="Submit">
```

## Use HTML according to its purpose.

Use elements (sometimes incorrectly called “tags”) for what they have been created for. For example, use heading elements for headings, `p` elements for paragraphs, `a` elements for anchors, etc.

``` 
<!-- Not recommended -->
<div onclick="goToRecommendations();">All recommendations

<!-- Recommended -->
<a href="recommendations/">All recommendations</a>
```

## Provide alternative contents for multimedia.

For multimedia, such as images, videos, animated objects via `canvas`, make sure to offer alternative access. For images that means use of meaningful alternative text (`alt`) and for video and audio transcripts and captions, if available.

``` 
<!-- Not recommended -->
<img src="spreadsheet.png">

<!-- Recommended -->
<img src="spreadsheet.png" alt="Spreadsheet screenshot.">
```

## Separate structure from presentation from behavior.

Don't use inline styling.

``` 
// correct 
 
// in CSS
.hidden {
  display: none;
}
 
// in HTML
<div class="hidden">ContentM
 
// NOT correct
<div style="display: none">Content
```

## Do not use entity references.

There is no need to use entity references like `&mdash;`, `&rdquo;`, or `&#x263a;`, assuming the same encoding (UTF-8) is used for files and editors as well as among teams.

The only exceptions apply to characters with special meaning in HTML (like `<` and `&`) as well as control or “invisible” characters (like no-break spaces).

``` 
<!-- Not recommended -->
The currency symbol for the Euro is &ldquo;&eur;&rdquo;.

<!-- Recommended -->
The currency symbol for the Euro is "€".
```

## Use a new line for every block, list, or table element, and indent every such child element.

``` 
// correct
<ul>
  <li>Item</li>
  <li>Item</li>
</ul>
 
// NOT correct
<ul>
<li>Item</li><li>Item</li>
</ul>
```

## Use \<button\> instead of \<a class="button"\>

In most cases when you wan't add a button class to an \<a\> element please think twice before doing it. Especially when there is another functionality on the element like: tooltip, dropdown, etc. We discovered that some touch devices has problems with handling \<a\> elements with empty href attribute

``` 
// correct
<li data-tooltip title="Add to basket">
    <button class="button">Add to basket</button>
</li>
 
// NOT correct
<li data-tooltip title="Add to basket">
    <a class="button" href="#">Add to basket</a>
</li>
```
