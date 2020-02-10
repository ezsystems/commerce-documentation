#  Content cache refresh. Technical implementation 

## General

### Slot implementation

Consult the [SignalSlots documentation](https://doc.ezplatform.com/en/latest/guide/signalslots/#signals-reference) from eZ Platform for more information about the SignalSlot system

silver.e-shop/src/Siso/Bundle/ToolsBundle/Service/ContentModificationQueueService

The ContentModificationSlot pushes content modification data into the queue by using the ContentModificationQueueService.

### Definitions

**Content modification** - array with information about modified content.

<table>
<thead>
<tr class="header">
<th>Attribute (key)</th>
<th>Value type</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>content_id</td>
<td>int</td>
<td>Content id of a modified content item.</td>
</tr>
<tr>
<td>action</td>
<td>string</td>
<td><p>Name of action which resulted in a content modification, e.g:</p>
<ul>
<li>publish</li>
<li>delete</li>
</ul></td>
</tr>
<tr>
<td>trigger</td>
<td>string</td>
<td>Legacy trigger name which caused a content modification, e.g:
<ul>
<li>post_publish</li>
<li>pre_delete</li>
</ul>
This attribute is creted only to provide additional information for content modification processor developers.</td>
</tr>
</tbody>
</table>

You can find a list of actions and triggers. See [supported content modification actions and triggers](#Contentcacherefresh.Technicalimplementation-SupportedActions)

Content modification processors can provide additional attributes. See getContentModificationAttributes.

**Content modification queue** - series of content modifications.

Implemented with Doctrine.

  - ContentModificationQueue - Doctrine entity (queue item).
  - ContentModificationQueueRepository - Doctrine entity repository for queue items.

**Content modification queue service** - service that stores content modifications in the queue and runs processors to handle content modifications.

**Content modification processor** - a service implemented to process information about content modification (e.g. refreshes related caches)

### Content modification queue service

ContentModificationQueueService has several responsibilities:

1.  Gather all services that are tagged with 'siso\_tools.content\_modification\_processor' (content modification processors).
2.  Fetch all current content modifications from the queue.
3.  Get additional attributes from processors.
4.  Push content modifications to the queue (table ses\_content\_modification\_queue).
5.  Pass every content modification object to every processor service.
6.  Remove processed content modifications from the queue.

![](attachments/23560554/23563512.png)

### Content modification processor

Every content modification processor must implement 2 actions:

1.  Get content id and expose additional attributes that processor can process later.  
    E.g. navigation cache processor retrieves a content object language code form the given content id. This information is being added to content modification data along with initial data.
2.  Process content modification data (e.g. clear cache).

#### Motivation. Why is it divided on 2 actions?

Content modifications are being stored in the content modification queue **immediately** after content is changed. However, processing of the queue doesn't happen immediately. Shop administrator can process the queue manually with a console script or configure a cronjob task.

It means that if user removes content item, there is no data which processor can use for clearing cache. For this reason, a content modification processor has to store information about removed content object before it is being processed.

Content modification processing is usually resource consuming operations. Therefore it should be done as a deferred task.

## How to implement new content modification processor

There are two steps to add your own Processor Service to the chain for content cache refresh.

### Step 1. Prepare service definition to add a service with the tag siso\_tools.content\_modification\_processor

``` 
<!-- your own content modification processor service -->
<service id="domain.yourname_content_modification_processor" class="%domain.yourname_content_modification_processor.class%">
    ...
    <tag name="siso_tools.content_modification_processor" />
</service>
```

### Step 2. Create processor service class, which has to implement interface ContentModificationProcessorServiceInterface

``` 
interface ContentModificationProcessorServiceInterface
{
    /**
     * Processes modified eZ content.
     *
     * @param ContentModificationQueueItemInterface[] $queue
     * @return void
     */
    function process($queue);

    /**
     * Get an array of attributes to use them later on the process stage.
     *
     * @param $contentId
     * @return array|null
     */
    function getContentModificationAttributes($contentId);
}
```

### Example

To understand how **getContentModificationAttributes** and **process** methods cooperate, let's see an example of TransContentModificationProcessorService which is responsible for clearing of translation caches.

**Excerpt of the method getContentModificationAttributes**  
``` 
function getContentModificationAttributes($contentId)
{
    // 1. Get services
    $contentService = $this->repository->getContentService();
    $content = $contentService->loadContent($contentId);
    $contentTypeService = $this->repository->getContentTypeService();

    // 2. Get data to store in attributes
    $contentType = $contentTypeService->loadContentType($content->contentInfo->contentTypeId);
    $languageCode = $content->getVersionInfo()->initialLanguageCode;

    // 3. Define array of attributes
    return array(
        'content_type' => $contentType->identifier,
        'language_code' => $languageCode,
    );
}
```

**Excerpt of process method**

``` 
public function process($queue)
{
    ...

    foreach($queue as $queueItem) {

        $data = $queueItem->getData();

        // Content type attribute is requested
        if (!isset($data['content_type']) || $data['content_type'] !== self::TRANSLATION_IDENTIFIER) {
            continue;
        }

        if(in_array($data['action'], array('delete','removetranslation', 'hide'))) {
            $this->transService->removeTranslationCache(
                ...
                // Language code attribute is requested
                $data['language_code']
            );
        }
    }
}
```

Please pay attention that process method uses attributes language\_code and content\_type, which were added to content modification array using method getContentModificationAttributes.

### <span id="Contentcacherefresh.Technicalimplementation-SupportedActions" class="confluence-anchor-link">Supported content modification actions and triggers

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th><div class="tablesorter-header-inner">
Action
</th>
<th><div class="tablesorter-header-inner">
Trigger
</th>
</tr>
</thead>
<tbody>
<tr>
<td>publish</td>
<td><pre><code>post_publish</code></pre></td>
</tr>
<tr>
<td>delete</td>
<td><pre><code>pre_delete</code></pre></td>
</tr>
<tr>
<td>hide</td>
<td><pre><code>post_hide</code></pre></td>
</tr>
<tr>
<td>move</td>
<td><pre><code>post_move</code></pre></td>
</tr>
<tr>
<td>swap</td>
<td><pre><code>post_swap</code></pre></td>
</tr>
<tr>
<td>updateobjectstate</td>
<td><pre><code>post_updateobjectstate</code></pre></td>
</tr>
<tr>
<td>updatepriority</td>
<td><pre><code>post_updatepriority</code></pre></td>
</tr>
<tr>
<td>addlocation</td>
<td><pre><code>post_addlocation</code></pre></td>
</tr>
<tr>
<td>removelocation</td>
<td><pre><code>pre_removelocation</code></pre></td>
</tr>
<tr>
<td>removetranslation</td>
<td><pre><code>pre_removetranslation</code></pre></td>
</tr>
</tbody>
</table>

## Implemented content modification processor services

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th>Service</th>
<th>Purpose</th>
</tr>
</thead>
<tbody>
<tr>
<td><pre><code>NavigationContentModificationProcessorService</code></pre></td>
<td>Clears navigation cache</td>
</tr>
<tr>
<td><pre><code>TransContentModificationProcessorService</code></pre></td>
<td>Clears translation cache</td>
</tr>
</tbody>
</table>

## Attachments:

![](images/icons/bullet_blue.gif) [Refresh cache sequence diagram.xml](attachments/23560554/23563354.xml) (application/drawio)  
![](images/icons/bullet_blue.gif) [Refresh cache sequence diagram.xml.png](attachments/23560554/23563365.png) (image/png)  
![](images/icons/bullet_blue.gif) [Refresh cache sequence diagram.xml](attachments/23560554/23563364.xml) (application/drawio)  
![](images/icons/bullet_blue.gif) [Refresh cache sequence diagram.xml.png](attachments/23560554/23563367.png) (image/png)  
![](images/icons/bullet_blue.gif) [Refresh cache sequence diagram.xml.png](attachments/23560554/23563355.png) (image/png)  
![](images/icons/bullet_blue.gif) [Refresh cache sequence diagram.xml](attachments/23560554/23563368.xml) (application/drawio)  
![](images/icons/bullet_blue.gif) [Copy of Refresh cache sequence diagram.xml](attachments/23560554/23563494.xml) (application/drawio)  
![](images/icons/bullet_blue.gif) [Copy of Refresh cache sequence diagram.xml.png](attachments/23560554/23563495.png) (image/png)  
![](images/icons/bullet_blue.gif) [Refresh Cache. Full sequence diagram.xml.png](attachments/23560554/23563347.png) (image/png)  
![](images/icons/bullet_blue.gif) [Refresh Cache. Full sequence diagram.xml](attachments/23560554/23563360.xml) (application/drawio)  
![](images/icons/bullet_blue.gif) [Refresh Cache. Full sequence diagram.xml](attachments/23560554/23563346.xml) (application/drawio)  
![](images/icons/bullet_blue.gif) [Refresh Cache. Full sequence diagram.xml.png](attachments/23560554/23563357.png) (image/png)  
![](images/icons/bullet_blue.gif) [Refresh Cache. Full sequence diagram.xml](attachments/23560554/23563356.xml) (application/drawio)  
![](images/icons/bullet_blue.gif) [Refresh Cache. Full sequence diagram.xml.png](attachments/23560554/23563359.png) (image/png)  
![](images/icons/bullet_blue.gif) [Refresh Cache. Full sequence diagram.xml.png](attachments/23560554/23563345.png) (image/png)  
![](images/icons/bullet_blue.gif) [Refresh Cache. Full sequence diagram.xml](attachments/23560554/23563366.xml) (application/drawio)  
![](images/icons/bullet_blue.gif) [Drawing1.xml.png](attachments/23560554/23563298.png) (image/png)  
![](images/icons/bullet_blue.gif) [Drawing1.xml](attachments/23560554/23563299.xml) (application/drawio)  
![](images/icons/bullet_blue.gif) [Drawing1.xml.png](attachments/23560554/23563300.png) (image/png)  
![](images/icons/bullet_blue.gif) [Drawing1.xml](attachments/23560554/23563358.xml) (application/drawio)  
![](images/icons/bullet_blue.gif) [Copy of Refresh cache sequence diagram.xml](attachments/23560554/23563492.xml) (application/drawio)  
![](images/icons/bullet_blue.gif) [Copy of Refresh cache sequence diagram.xml.png](attachments/23560554/23563509.png) (image/png)  
![](images/icons/bullet_blue.gif) [Copy of Refresh cache sequence diagram.xml](attachments/23560554/23563297.xml) (application/drawio)  
![](images/icons/bullet_blue.gif) [Copy of Refresh cache sequence diagram.xml.png](attachments/23560554/23563496.png) (image/png)  
![](images/icons/bullet_blue.gif) [Extended seq diagramm.xml](attachments/23560554/23563289.xml) (application/drawio)  
![](images/icons/bullet_blue.gif) [Extended seq diagramm.xml.png](attachments/23560554/23563304.png) (image/png)  
![](images/icons/bullet_blue.gif) [Extended seq diagramm.xml](attachments/23560554/23563303.xml) (application/drawio)  
![](images/icons/bullet_blue.gif) [Extended seq diagramm.xml.png](attachments/23560554/23563302.png) (image/png)  
![](images/icons/bullet_blue.gif) [Extended seq diagramm.xml](attachments/23560554/23563301.xml) (application/drawio)  
![](images/icons/bullet_blue.gif) [Extended seq diagramm.xml.png](attachments/23560554/23563497.png) (image/png)  
![](images/icons/bullet_blue.gif) [Extended seq diagramm.xml](attachments/23560554/23563498.xml) (application/drawio)  
![](images/icons/bullet_blue.gif) [Extended seq diagramm.xml.png](attachments/23560554/23563511.png) (image/png)  
![](images/icons/bullet_blue.gif) [Extended seq diagramm.xml](attachments/23560554/23563508.xml) (application/drawio)  
![](images/icons/bullet_blue.gif) [Extended seq diagramm.xml.png](attachments/23560554/23563290.png) (image/png)  
![](images/icons/bullet_blue.gif) [Extended seq diagramm.png](attachments/23560554/23563512.png) (image/png)  
