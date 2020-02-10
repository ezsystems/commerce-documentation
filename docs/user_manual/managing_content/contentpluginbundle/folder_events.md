#  Folder (Events) 

Special folder to show events.

![](attachments/23561064/23563099.png)

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

The editor can create folders inside this folder to organize events (e.g. fairs or workshops).  
Events will be fetched with a depth of two and shown grouped by folder.  
If the events are grouped a inside navigation with anchors to the groups will be visible on top of the page.

Events will be fetched only if the "End date" is today or newer.

**Layout:** list view  
**Depth:** 2  
**Classes:** event  
**Items per page:** 10  
**Sort by:** "Start date" (ascending)

![](attachments/23561064/23563109.png)

### Create

You can create a folder everywhere in the content structure.

## Attachments:

![](images/icons/bullet_blue.gif) [folder\_news.png](attachments/23561064/23563097.png) (image/png)  
![](images/icons/bullet_blue.gif) [folder\_all\_content.png](attachments/23561064/23563098.png) (image/png)  
![](images/icons/bullet_blue.gif) [folder\_events.png](attachments/23561064/23563099.png) (image/png)  
![](images/icons/bullet_blue.gif) [folder\_news\_backend.png](attachments/23561064/23563100.png) (image/png)  
![](images/icons/bullet_blue.gif) [article\_backend.png](attachments/23561064/23563101.png) (image/png)  
![](images/icons/bullet_blue.gif) [article.png](attachments/23561064/23563102.png) (image/png)  
![](images/icons/bullet_blue.gif) [folder\_backend.png](attachments/23561064/23563103.png) (image/png)  
![](images/icons/bullet_blue.gif) [article\_list.png](attachments/23561064/23563104.png) (image/png)  
![](images/icons/bullet_blue.gif) [folder\_events\_backend.png](attachments/23561064/23563109.png) (image/png)  
