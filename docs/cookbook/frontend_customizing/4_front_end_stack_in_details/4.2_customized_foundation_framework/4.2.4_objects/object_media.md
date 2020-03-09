# Object - Media

The media object can save 100 lines of code.

If you are interested in details about media object please refer to these articles:

- http://www.stubbornella.org/content/2010/06/25/the-media-object-saves-hundreds-of-lines-of-code/

Media object is not part of standard front-end. In order to enable it please uncomment it in Objects section in `scss/style.scss` file.

## Sass

**File location**

``` 
scss/storm/_objects.media.scss
```

### Available classes

``` 
.o-media
.o-media__img
.o-media__body
```

## HTML

### Basic usage

``` 
<div class="o-media">
  <div class="o-media__img">
    <img src="http://placehold.it/100x100" alt="" />
  
  <p class="o-media__body">
    Lorem ipsum dolor sit amet, consectetur adipisicing elit. Cum in facere enim dolorem at aliquid laborum rem molestiae eos, assumenda adipisci eveniet velit iure quibusdam, deleniti rerum iste culpa nesciunt!
  </p>

```
