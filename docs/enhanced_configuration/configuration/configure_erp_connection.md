#  Configure ERP connection 

By default eZ Commerce uses the TsoWebconnectorLayer which enables the shop to communicate with a Microsoft Dynamics NAV System.

## Requirements

  - An ERP System Microsoft Dynamics NAV  and the TSO Webconnector product installed.
  - The main URL to the Webservice 

## Configuration

Configure the ERP connection in the backend:

![](attachments/23560216/23570776.png)

Please check the configuration if ERp is enabled:

``` 
siso_local_order_management.default.send_order_to_erp: true
    siso_order_history.default.use_local_documents: false
```

## Attachments:

![](images/icons/bullet_blue.gif) [image2019-5-16\_10-2-51.png](attachments/23560216/23570776.png) (image/png)  
