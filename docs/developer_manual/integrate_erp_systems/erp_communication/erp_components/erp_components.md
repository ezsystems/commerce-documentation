#  ERP Components 

## Components of the ERP module

The ERP module roughly consists of four main components. These components are not reflected by the namespace since most of the implementation is bundled in Symfony services and thus is located within the namespace 'Services'.

The components are:

  - Service
    
      - The bundle provides a simple one-class API to the standard methods. Although this is also a Symfony service, it does not mean that all Symfony services of this bundle are placed within that component.

  - Messages
    
      - This component covers all classes, that handle with the messages which are used to communicate with the ERP system.
    
      - Includes an Event-Observer pattern which provides a hook into the message creation and data feeding.

  - Transport
    
      - The transport component is responsible for the serialization and network communication to the remote system.

  - Mapping
    
      - The mapping API is used to transform a specific message format into another. This may be necessary if the remote system expects a different data structure than the one which is used in the eZ Commerce Advanced.

The general process looks like this:

1.  Invocation of one of the ERP service methods. The service method accepts all the data that is necessary for the ERP request and does the complete process of combines all following steps, that are necessary for the communication.

2.  The messages component creates the respective message object which is then fed with the data of the request. During this step an event is dispatched to registered observers, which may add, change or remove data from the message object.

3.  The message object is given to the transport component, which:
    
    1.  serializes the data,
    
    2.  optionally maps the serialized structure into the targeted format,
    
    3.  sends it to the ERP remote system, which may be a web-service, specifically the Web-Connector, in the most cases.

4.  After the remote system responded, the steps are performed in reverse order back to the ERP service method.

5.  The service method returns the response in the way, which is defined by the method, for example a specific class instance.
