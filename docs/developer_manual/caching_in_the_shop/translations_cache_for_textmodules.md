#  Translation cache for Textmodules (st\_textmodule) 

Consult the [SignalSlots documentation](https://doc.ezplatform.com/en/latest/guide/signalslots/#signals-reference) from eZ Platform for more information about the SignalSlot system

# RemoveTranslationCacheSlot

### General

The RemoveTranslationCacheSlot is used to remove translations of the st\_textmodules immediately after they have been updated, deleted, created, moved to or recovered from trash

The RemoveTranslationCacheSlot accepts only the following signals (other signals are ignored by this particular slot):

**silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Slot/RemoveTranslationCacheSlot.php**

``` 
Signal\ContentService\UpdateContentSignal
Signal\ContentService\DeleteContentSignal
Signal\ContentService\CreateContentSignal
Signal\TrashService\TrashSignal
Signal\TrashService\RecoverSignal
```
