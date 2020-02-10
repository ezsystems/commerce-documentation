#  Exception Handling 

# Exception Listener

eZ Commerce provides default exception handling. Every not catched exception will be handled by

    Silversolutions\Bundle\EshopBundle\EventListener\ExceptionListener.

  - Exceptions will be logged and displayed also in the page header as custom header: 'X-Logged-Exception`'`

### Configuration

If exception is thrown, user will see just little information on the page. If you want to see the complete exception, please add your environment into configuration.

``` 
parameters:
    #add environments, where you want to display the debug information
    siso_core.debug_environments: ['local', 'dev']
```

#### Complete exception in 'dev' environment.

![](attachments/23560498/23563276.png)

#### Exception in 'prod' environment

![](attachments/23560498/23563274.png)

## Attachments:

![](images/icons/bullet_blue.gif) [Bildschirmfoto 2015-08-05 um 09.08.33.png](attachments/23560498/23563276.png) (image/png)  
![](images/icons/bullet_blue.gif) [Bildschirmfoto 2015-08-05 um 09.09.53.png](attachments/23560498/23563274.png) (image/png)  
