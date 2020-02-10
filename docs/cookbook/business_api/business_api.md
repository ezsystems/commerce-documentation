#  Business Api 

The BusinessApi is the layer between the application entry points (like controllers or CLI commands) and the particular services. The following picture is a simple version of this architecture:

### Application Flow

![](attachments/23560523/23563983.png)
To access the **BusinessApi**, you have to make usage of the [BusinessApi Invocation Service](BusinessApi-Invocation-Service_23560513.html). 

#### Application Flow

  - The Business API Operations are working with Input - Output Data.
  - The Business API invocation service call the Operation service with Input Data
  - The Business API invocation service returns Output Data

## Input/Output Data

All data passed to the BusinessApi calls (input)  must be an instance of a class, that extends [ValueObject](ValueObject_23560594.html). This input data class should define all fields and/or structs, which are needed to process the called business logic. But business logic must not be implemented in these classes.

Also all data, which is returned by the BusinessApi calls (output), must be an instance of the ValueObject class.

All the Input and Output classes are located in here:

| type   | location                                                                                                 |
| ------ | -------------------------------------------------------------------------------------------------------- |
| Input  | Silversolutions\\Bundle\\EshopBundle\\Entities\\BusinessLayer\\InputValueObjects\\InputValueObjects\\\*  |
| Output | Silversolutions\\Bundle\\EshopBundle\\Entities\\BusinessLayer\\InputValueObjects\\OutputValueObjects\\\* |

Output classes are built accordingly.

## Attachments:

![](images/icons/bullet_blue.gif) [Bildschirmfoto 2013-12-13 um 16.04.31.png](attachments/23560523/23563983.png) (image/png)  
