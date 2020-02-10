#  Folder (News) 

Special folder to show news.

![](attachments/23561065/23563118.png)

## Content class

Category

Name

Identifier

Type

Description

Content

Internal name

internal\_name

Text line

Internal name will be used and shown only in the backend (recommended for multilingual environments)

Title

title

Text line

Title will be shown in navigation, breadcrumb and on detailpages. (also used as fallback for landingpages and listpages)

Intro

intro

XML block

Intro text will be shown on detailpages. (also used as fallback for landingpages and listpages)

Body

body

XML block

Body text will be shown on detailpages.

Media

media

Object relations

Images and videos for detailpages (also used as fallback for landingpages and listpages)

Layout

layout

Layout

eZ Flow blocks and elements
Landingpages & Listpages

Title

alternative\_title

Text line

This title will be shown on landingpages or listpages (if empty the common title will be used)

Intro

alternative\_intro

XML block

This intro text will be shown on landingpages or listpages (if empty the common intro text will be used)

Image

alternative\_image

Object relation

This image will be shown on landingpages or listpages (if empty the first image of the "Media" attribute will be used)

Meta

Meta title

meta\_title

Text line

Meta title for SEO

Meta description

meta\_description

Text line

Meta description for SEO

Meta keywords

meta\_keywords

Text line

Meta keywords for SEO

## Definition of what will be fetched and shown.

There is a special list definition configured in configuration files.

News will be shown only if the "Date" is today or newer.

**Layout:** list view  
**Depth:** 1  
**Classes:** news  
**Items per page:** 10  
**Sort by:** "Date" (descending)

![](attachments/23561065/23563117.png)

### Create

You can create a folder everywhere in the content structure.

## Attachments:

![](images/icons/bullet_blue.gif) [article.png](attachments/23561065/23563112.png) (image/png)  
![](images/icons/bullet_blue.gif) [folder\_news\_backend.png](attachments/23561065/23563117.png) (image/png)  
![](images/icons/bullet_blue.gif) [folder\_news.png](attachments/23561065/23563118.png) (image/png)  
![](images/icons/bullet_blue.gif) [article\_list.png](attachments/23561065/23563119.png) (image/png)  
![](images/icons/bullet_blue.gif) [article\_backend.png](attachments/23561065/23563120.png) (image/png)  
![](images/icons/bullet_blue.gif) [folder\_all\_content.png](attachments/23561065/23563121.png) (image/png)  
![](images/icons/bullet_blue.gif) [folder\_backend.png](attachments/23561065/23563122.png) (image/png)  
