# Business API

The BusinessApi is the layer between the application entry points (like controllers or CLI commands) and the particular services. The following picture is a simple version of this architecture:

### Application Flow

![](../img/business_api_1.png)

To access the **BusinessApi**, you have to make usage of the [BusinessApi Invocation Service](businessapi_invocation_service.md). 

#### Application Flow

- The Business API Operations are working with Input - Output Data.
- The Business API invocation service call the Operation service with Input Data
- The Business API invocation service returns Output Data

## Input/Output Data

All data passed to the BusinessApi calls (input)  must be an instance of a class, that extends [ValueObject](../valueobject.md). This input data class should define all fields and/or structs, which are needed to process the called business logic. But business logic must not be implemented in these classes.

Also all data, which is returned by the BusinessApi calls (output), must be an instance of the ValueObject class.

All the Input and Output classes are located in here:

| type   | location                                                                                                 |
| ------ | -------------------------------------------------------------------------------------------------------- |
| Input  | Silversolutions\\Bundle\\EshopBundle\\Entities\\BusinessLayer\\InputValueObjects\\InputValueObjects\\\*  |
| Output | Silversolutions\\Bundle\\EshopBundle\\Entities\\BusinessLayer\\InputValueObjects\\OutputValueObjects\\\* |

Output classes are built accordingly.
