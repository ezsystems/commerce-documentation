# Templates for catalog

The controller tries to determine a internal URL using the URL service. The request will be forwarded to the `CatalogService` and the service will provide a the corresponding `catalogElement`.

Choosing a template for rendering

The controller uses the following logic to select the template for rendering the product or catalog:

- The method `getViewType()` provides the name of the class (e.g. `CatalogNode` or `OrderableProductnode`)
- The class name is used to to identify the corresponding template. This can be done by a configuration setting (in `silver.eshop.yml`)
- The configuration file will provide a template name for each class

### Parameters provided in the template

The following parameter will be passed to the template:

| Parameter        | Meaning                                 |
| ---------------- | --------------------------------------- |
| `catalogElement` | The data of the `CatalogElement`        |
| `parent`         | The data of the parent `CatalogElement` |
