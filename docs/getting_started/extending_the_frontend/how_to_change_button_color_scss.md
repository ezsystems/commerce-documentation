# How to: Change button color (scss)

Before you start with this example you have to make sure to finish the [preparation](frontend_workflow_with_sass.md) topic.

## Introduction

With [SCSS](https://sass-lang.com/) you have the possibility to adjust variables that influences the Shop Layout. In this example we want to change the button color for the whole shop. With SCSS it is super easy to change that. We just have to find the Variable for the button background color in the settings file.

## SCSS settings

We are using 4 setting files to adjust the basic design of the shop frontend. You can find them in `app/Resources/public/scss/settings/`

- `_colors.flat.scss` - This file contains color variables we are using in the shop. If you need more colors just add them here.
- `_global.scss` - This file contains some global variables you need in every component. Sometimes you have to define a variable here to make it work globally.
- `_foundation.scss` - This is the main settings file for the foundation framework. In this file you can define the basic settings for the framework and customize it.
- `_storm.scss` - This file contains all the settings we have created for using our shop components to extend the foundation framework with eCommerce related stuff or things we are using in the shop frontend

## Guide to change the button background variable

As we are using basic Foundation components e.g. the button classes. We will now open the `_foundation.scss` file to adjust the basic color of the buttons in our shop.

1.  Look for the variable with the name `$button-bg-color`, right now it has another variable assigned called "$flat-peter-river" that comes from the `_colors.flat.scss` file
2.  Let's change the button background color to green.
3.  Change the line to `$button-bg-color: \#2ed573;`
4.  As we are using SCSS we have to now start the CSS building process with gulp and assetic we have learned in the [preparation](frontend_workflow_with_sass.md) topic
5.  After you have run gulp and assetic you can now see that the whole shop is using the new button background color for the basic buttons.

!!! note

    If you dont see any changes please remove the cache!

## For your interest:

Please feel free to adjust or extend the framework with your own Components. It should be possible to adjust a lot of things with the settings files.
