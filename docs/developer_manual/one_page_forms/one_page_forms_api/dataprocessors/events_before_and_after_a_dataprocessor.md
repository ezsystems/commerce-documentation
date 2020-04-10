# Events before and after DataProcessor

To be able to extend the logic of the DataProcessors, you can listen to two events for each DataProcessor. Before and after the execution, an event is triggered.

## Event Names

The name of the event is put together dynamically by a prefix and a suffix.

#### Prefix

- The prefix for the event before the execution is: `ses_pre_execute_`
- The prefix for the event after the execution is: `ses_post_execute_`

This value is stored in the class: `Silversolutions\Bundle\EshopBundle\Event\DataProcessor\DataProcessorEvents`

#### Suffix

The suffix is the ID of the DataProcessor service. An example for the service `ses_forms.create_ez_user` is:

- `ses_pre_execute_ses_forms.create_ez_user`
- `ses_post_execute_ses_forms.create_ez_user`

To listen to one of this events, you have to define a service and tag it like this:

``` xml
<service id="ses_data_processor.pre_execute.create_ez_user_handler" class="%ses_data_processor.pre_execute.create_ez_user_handler.class%">
      <tag name="kernel.event_listener" event="ses_pre_execute_ses_forms.create_ez_user" method="preExecute" />
      <tag name="kernel.event_listener" event="ses_post_execute_ses_forms.create_ez_user" method="postExecute" />
</service>
```

Beside the event, you want to listen to, make sure you implement the methods in the corresponding class.

Of course, you have to implement this class and you can inject dependencies into.

Now, you can implement your class where you can access and manipulate the normalized FormEntity like this:

``` php
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

If you manipulate the normalized entity, please note that it will effect the next DataProcessors.
