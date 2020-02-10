#  EzContentBreadcrumbsGenerator 

This generator renders the breadcrumbs for standard eZ Platform content.  

|||
|-------------| -------------- |
| FQN | Silversolutions\\Bundle\\EshopBundle\\Service\\Breadcrumbs\\EzContentBreadcrumbsGenerator|
| Criterion for responsibility   | The generator triggers if the session attribute 'location' is set. This is set by some eZ routine in the case the router matched eZ content. |
| Notes to the rendering process | Renders all elements of the path of the currently displayed content location as breadcrumbs, up to the eZ content root.|
