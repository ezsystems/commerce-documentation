# Supported Field Types for products in eZ

In the `dataMap` the shop provides Fields of the following Field Types:

- TextLine
- RichText
- Image
- BinaryFile
- Checkbox
- RelationList
- SpecificationsType (for `specificationdata` only)
- VariantType (for `variants` only)

Attributes using other Field Types are skipped. 

If you want to add more Field Types then the `eZ5Catalogfactory` (`fillCatalogElementDataMap`) has to be extended.
