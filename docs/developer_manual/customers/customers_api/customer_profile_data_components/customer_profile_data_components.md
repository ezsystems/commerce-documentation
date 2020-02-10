#  Customer profile data components 

## Components

The complete model of the customer profile data consists of following components

| Component       | Description                                                                                                                                                                                                                                                                                                                     | Location                                        |
| --------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------- |
| `Model`         | The customer profile data itself. This is the object, which holds all user profile related data such as first name, last name, customer number, contact number, addresses and the like. Instances of this model only have these data and therefore act as a *data entity* within the system.                                    | `EshopBundle/Model/CustomerProfileData`         |
| `Services`      | Services to create profile (model) instances. Provides also a mapping service for the [deprecated model](/pages/createpage.action?spaceKey=EZC14&title=Customer+data+silver.e-shop+3.0&linkCreation=true&fromPageId=23560900) to avoid compatibility problems of other components depending on the old model within the system. | `EshopBundle/Services/CustomerProfileData`      |
| `Event`         | Provides the profile event definitions. Events are thrown on certain circumstances (e.g. before or after a customer profile is fetched).                                                                                                                                                                                        | `EshopBundle/Event/CustomerProfileData`         |
| `EventListener` | Provides the implementations of the actions, which are triggered, when an event is thrown. This way, we have an elegant possibility of modifying profile data (e.g. what happens to the profile data, when a data source like the ERP is unavailable, fallback for something?).                                                 | `EshopBundle/EventListener/CustomerProfileData` |

## Fundamental principle

Unlike the deprecated model, the customer profile data are **strictly** separated into the components described above. An instance of a profile model or a listener will **never** fetch data itself, instead **always** the service is used to enrich or modify profile data.

Following this principle, we always have a customer profile instance within our request as a *data entity*. We always have one or more profile services to retrieve (fetch) profile data entities. All events that are thrown within the profile services are handled by listeners. These listeners do actions on the profile data entities under explicit conditions using always the profile services.
