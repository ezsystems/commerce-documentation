# Utility - Typography

We use this utility to manipulate font properties.

## Sass

``` 
scss/storm/_utils.typo.scss
```

### Available classes

**Font size properties**

``` 
.u-text-tiny
.u-text-smaller
.u-text-small
.u-text-big
.u-text-bigger
.u-text-huge
```

| Class           | Font size |
| --------------- | --------- |
| .u-text-tiny    | 50%       |
| .u-text-smaller | 80%       |
| .u-text-small   | 90%       |
| .u-text-big     | 125%      |
| .u-text-bigger  | 150%      |
| .u-text-huge    | 200%      |

**Text transform properties**

``` 
.u-text-no-transform
.u-text-uppercase
.u-text-lowercase
```

## HTML

**Make bigger font size**

``` 
<div class="... ... u-text-bigger">
...

```

It will set the font-size property to 125% of it's original. So if the original is 16px, after applying this class it will be 20px.

Different text transform

``` 
// lowercase
<div class="... ... u-text-lowercase">
...
// uppercase
<div class="... ... u-text-uppercase">
...
// reset to default text-transform
<div class="... ... u-text-no-transform">
...

```
