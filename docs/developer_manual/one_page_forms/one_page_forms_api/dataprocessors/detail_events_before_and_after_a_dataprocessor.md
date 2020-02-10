#  Detail: Events before and after a DataProcessor 

To be able to extend the logic of the DataProcessors, you can listen to two events for each DataProcessor. Before and after the execution, an event is triggered.

## Event Names

The name of the event is put together dynamically by a prefix and a suffix.

#### Prefix

  - The prefix for the event before the execution is: ses\_pre\_execute\_
  - The prefix for the event after the execution is: ses\_post\_execute\_

This value is stored in the class: **Silversolutions\\Bundle\\EshopBundle\\Event\\DataProcessor\\DataProcessorEvents**

#### **Suffix**

The suffix is the ID of the DataProcessor-service. An example for the service **ses\_forms.create\_ez\_user** is:

  - ses\_pre\_execute\_ses\_forms.create\_ez\_user
  - ses\_post\_execute\_ses\_forms.create\_ez\_user

To listen to one of this events, you have to define a service and tag it like this:

``` 
<service id="ses_data_processor.pre_execute.create_ez_user_handler" class="%ses_data_processor.pre_execute.create_ez_user_handler.class%">
      <tag name="kernel.event_listener" event="ses_pre_execute_ses_forms.create_ez_user" method="preExecute" />
      <tag name="kernel.event_listener" event="ses_post_execute_ses_forms.create_ez_user" method="postExecute" />
</service>
```

Beside the event, you want to listen to, make sure you implement the methods in the corresponding class.

Of course, you have to implement this class and you can inject dependencies into.

Now, you can implement your class where you can access and manipulate the normalized FormEntity like this:

``` 
use Silversolutions\Bundle\EshopBundle\Entities\Forms\Normalize\Entity;
use Silversolutions\Bundle\EshopBundle\Event\DataProcessor\PreDataProcessorExecuteEvent;
use Silversolutions\Bundle\EshopBundle\Event\DataProcessor\PostDataProcessorExecuteEvent;

class EzCreateUserEventHandler
{
    public function preExecute(PreDataProcessorExecuteEvent $event)
    {        
        /** @var Entity $entity */
        $entity = $event->getNormalizedEntity();   
        ...   
    }
 
    public function postExecute(PostDataProcessorExecuteEvent $event)
    {        
        /** @var Entity $entity */
        $entity = $event->getNormalizedEntity();      
        ...
    }
}
```

If you manipulate the normalized entity, please note that it will effect the next DataProcessors\!

To learn more about the normalized FormEntity, please see here:

  - [Detail: Mapping with Annotations](/pages/createpage.action?spaceKey=EZC14&title=Detail%3A+Mapping+with+Annotations&linkCreation=true&fromPageId=23560592)

To get an overview about all DataProcessors, please see here:

  -  [DataProcessors](DataProcessors_23560504.html)
