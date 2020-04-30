# Supported Field Types for products in eZ

The shop provides fields from eZ in the dataMap having these fieldtypes:

- TextLine
- RichText
- Image
- BinaryFileValue
- CheckboxValue
- RelationListValue
- SpecificationsType (for specificationdata only)
- VariantType (for variants only)

Attributes using other Fieldtypes will be skipped. 

If you want to add more FieldTypes then the eZ5Catalogfactory (fillCatalogElementDataMap) has to be extendedfillCatalogElementDataMap) has to be extended.
