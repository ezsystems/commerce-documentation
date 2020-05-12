# Templates for catalog

The controller tries to determine an internal URL using the URL service.
The request is forwarded to `CatalogService` and the service provides the corresponding `catalogElement`.

## Choosing a template for rendering

The controller uses the following logic to select the template for rendering the product or catalog:

- The method `getViewType()` provides the name of the class (e.g. `CatalogNode` or `OrderableProductnode`)
- The class name is used to identify the corresponding template. This can be done by a configuration setting (in `silver.eshop.yml`)
- The configuration file provides a template name for each class

## Parameters provided in the template

The following parameters are passed to the template:

| Parameter        | Meaning                                 |
| ---------------- | --------------------------------------- |
| `catalogElement` | The data of the `CatalogElement`        |
| `parent`         | The data of the parent `CatalogElement` |
