# How to import products (API)

## Intro

- Econtent repository: helper methods for automatically generating metadata.
- catalog data (categories, products and related attributes) need to be stored individually for every language used in the shop
- available classes for new database objects can be obtained from the *sve\_class* table (by default only two classes: 1 = category, 2 = product)

## Attributes

Every catalog class defines a number of valid attributes for their elements. The name identifier, id and data type of all attributes are stored in the table *sve\_class\_attributes*.

- required fields: node_id of product, attribute id and language code

After a new product has been created, attributes can be associated with the product using its node\_id. In addition, creating a new attribute requires its id and the language code.

Attributes use three distinct data types to store their data (text, float and int). The API provides a corresponding setter method for every data type that needs to be called explicitly on import.

|Data type|Method|
|--- |--- |
|float|setDataFloat|
|int|setDataInt|
|text/string|setDataText|

## How to import a category

- class_id = 1
- categories are used to group products by setting the parent_id of the products to the node_id of the category
- the root node of the tree is 2 (will be the parent of first category)

**Example - Import a new category**

``` php
$categoryName = 'My category';

$econtentCategory = new SveObject();
$econtentCategory->setClassId(1); // 2 = product, 1 = category
$econtentCategory->setParentId(2); // root node
$econtentCategory->setBlocked(false);
$econtentCategory->setHidden(false);

$econtentCategory->setChangeDate(new \DateTime());
$objectRepo->generateMetaData($econtentCategory, $categoryNodeId, $categoryName);
$this->em->persist($econtentCategory);
$this->em->flush();
// import an attribute for the new category
$econtentAttribute = new SveObjectAttributes();
$econtentAttribute->setNodeId($nodeId);
$econtentAttribute->setAttributeId(101); // 201 = ses_name
$econtentAttribute->setLanguage('eng-GB');

$econtentAttribute->setDataText($categoryName);
$this->em->persist($econtentAttribute);
$this->em->flush();
```

## How to import a product

- class_id = 2

Creating a product requires a parent-id (its location in the product tree/catalog). Changing the parent\_id will move the product in the tree. After creation, the products' node\_id will serve as its unique identifier.

- product entities provide getters and setters for their attributes
- for imports in the \*\_tmp table: $nodeId = $objectRepo-\>getNextNodeId();
- use the method generateMetaData to generate metadata such as the url alias of the product

## Example

- create product object in db and set properties
- create attributes and associate them with the object
    - different attributes have different data types (text is default)

``` php
// get the doctrine repository
$objectRepo = $this->em->getRepository(SveObject::class);

// create a category
$categoryNodeId = $objectRepo->getNextNodeId();

$econtentCategory = new SveObject();
$econtentCategory->setClassId(1); // 2 = product, 1 = category
$econtentCategory->setParentId(2); // root node
$econtentCategory->setBlocked(false);
$econtentCategory->setHidden(false);

$econtentCategory->setChangeDate(new \DateTime());
$objectRepo->generateMetaData($econtentCategory, $categoryNodeId, 'Products');

$this->em->persist($econtentCategory);
$this->em->flush();

// create a product and set its data fields
$productNodeId = $objectRepo->getNextNodeId();

$econtentProduct = new SveObject();
$econtentProduct->setClassId(2); // 2 = product, 1 = category
$econtentProduct->setParentId(3); // node_id of the category
$econtentProduct->setMainNodeId($productNodeId);
$econtentProduct->setBlocked(false);
$econtentProduct->setHidden(false);

$econtentProduct->setChangeDate(new \DateTime());
$objectRepo->generateMetaData($econtentProduct, $productNodeId, 'New Product');

$this->em->persist($econtentProduct);
$this->em->flush();

// import an attribute for the new product
$econtentAttribute = new SveObjectAttributes();
$econtentAttribute->setNodeId($nodeId);
$econtentAttribute->setAttributeId(201); // 201 = ses_name
$econtentAttribute->setLanguage('eng-GB');

$econtentAttribute->setDataText((string)'product name');

$this->em->persist($econtentAttribute);
$this->em->flush();
```

Create the index for the new products (tmp area):

``` 
php bin/console silversolutions:indexecontent
```

After the the import the DB tables and solr cores have to be switched:

``` 
php bin/console silversolutions:indexecontent swap
php bin/console silversolutions:econtent-tables-swap
```

Details see [econtent - Staging system](../../econtent_features/econtent_staging_system.md) and [Indexing econtent data](../../econtent_features/indexing_econtent_data/indexing_econtent_data.md).
