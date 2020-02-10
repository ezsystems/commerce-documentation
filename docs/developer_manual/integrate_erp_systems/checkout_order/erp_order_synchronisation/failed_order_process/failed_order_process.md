#  Failed Order Process 

## Failed Order Process

This is connected to the order submission. For usability reasons, it is not advisable to let the whole checkout fail if the communication to the ERP system is somehow broken. When the submission of an order to the ERP system fails, the order must be stored somewhere for a later retry.

## The OrderFailedEvent

FQN: `\Silversolutions\Bundle\EshopBundle\Event\Erp\OrderFailedEvent`

Event identifier: **`siso_erp.order_failed`** as defined in `\Silversolutions\Bundle\EshopBundle\Event\Erp\ErpEvents::ORDER_FAILED`

This event is dispatched by the WebConnectorErpService if an error occurs during the communication with the remote ERP system. It SHOULD also be dispatched by other implementations of the AbstractErpService under similar circumstances.

If the class attribute $maxCount is not set or null, listeners for this event will ignore a maximum number of retries for failed / lost orders.

The order will be submitted again (for backend manipulations of orders).

## ErpErrorServiceInterface

FQN: `\Silversolutions\Bundle\EshopBundle\Services\ErpErrorServiceInterface`

The interface declares a method, which is intended to process failed orders:

  - **processFailedOrder**(*Basket* $order, *AbstractMessage* $message, $maxCount = null) : *void*

### Standard implementation

FQN: `\Silversolutions\Bundle\EshopBundle\Services\StandardErpErrorService`

Service ID: `siso_erp.erp_error_service`

Currently, the interface is only implemented in an event listener. This listener is subscribed to the OrderFailedEvent and set up with the highest priority.

As this service is providing some crucial information to the basket object (as ERP error message and fail counter) it is recommended to have it the highest priority in all setups.

The standard implementation uses the Basket states in order to realize the queue of failed orders. Orders with the state `ordered_failed` are considered to be queued.

The number of failed tries is stored in the Basket entity's attribute $erpFailCounter. The maximum allowed number of tries is configured using eZ's ConfigResolver concept, e.g.: `siso_checkout.default.max_failed_order: 3`

Only basket objects with the states `payed` and `ordered_failed` are expected to be transmitted as order and thus are allowed to be processed as failed order. If the given basket object has a different state, a RuntimeException is thrown.

## OrderFailedNotifyListener

FQN: `\Silversolutions\Bundle\EshopBundle\EventListener\Erp\OrderFailedNotifyListener`

Service ID: `siso_eshop.order_failed_listener`

If an order failed, an administrator is informed about it by sending an e-mail. For that purpose an additional listener exists for the **`siso_erp.order_failed`** event. This listener has a lower priority than the `StandardErpErrorService`, which ensures that the ERP error message is already set in the basket (given by the event object).

The following container parameters are passed to the service:

  - ses\_swiftmailer
  - siso\_eshop.order\_failed.subject

Thus, the mailReceiver subelement under ses\_swiftmailer is used as the recipient for the notification.
