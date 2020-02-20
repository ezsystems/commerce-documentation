# Econtent dataprovider - How to extend EcontentCatalogFactory class

Catalog objects are created using EcontentCatalogFactory class. This involves both product and product groups, or other type of elements which could be defined.

Sometimes in projects is needed to extend this class to provide additional functionality or logic to the catalog elements.

Here we will do a step by step extension of EcontentCatalogFactory to add some custom functionality.

Please note that Catalog objects have some default properties + a data map with additional fields. This data map is very flexible and allows a very easy way to add new custom properties to objects. A custom property could anything you want, like a file object, an array of files, images, custom values based on standard values, flags, etc.

In these example we will extend EcontentCatalogFactory to add a video to the product data map.

## Step 1: Create the new class that will extend EcontentCatalogFactory

We will use the following name: MyProjectEcontentCatalogFactory

Create the class file in your project bundle directory.

``` php
<?php
/
namespace MyProject\Bundle\ProjectBundle\Services\Factory;
 
class MyProjectEcontentCatalogFactory extends EcontentCatalogFactory
{
  
}
```

## Step 2: Modify your services.yml file and add the new class you just created.

Please note that if you are not overriding the constructor, it is enough to add only the parameter with the original key and the new class.

```
<parameter key="silver_catalog.econtent_catalog_factory.class">MyProject\Bundle\ProjectBundle\Services\Factory\CornelsenEcontentCatalogFactory</parameter>
```

## Step 3: Create a new extract method for new video.

In this case we will assume that the object has one video and its name is the sku + mp4 extension.

Note:

Catalog elements have a data map, which is an array of additional properties. Every new element that is added to the catalog element should be placed in its data map. The catalog element data map supports different Field objects as its elements.

Available Field elements could be found here: EshopBundle/Content/Fields

For this example we will use the Field Field object.

We also use symphony finder object to support file system access.

For more info on finder you can check:

http://symfony.com/doc/current/components/finder.html

``` php
/**
 * This method will extract all videos of specified extensions.
 *
 * @param $fieldIdentifier
 * @param $dataMap
 * @return FileField
 */
protected function extractVideo($fieldIdentifier, $dataMap)
{
    $attributes = $this->getFieldFromDataMap($fieldIdentifier, $dataMap);
    $fileName = $attributes['data_text'].'mp4';
    $finder = new Finder();
    try {
        $finder->files()->in('videos/')->name($fileName);
        /** @var \Symfony\Component\Finder\SplFileInfo $file */
        foreach ($finder as $file) {
            $fileName = $file->getRelativePathname();
            $fileSize = $file->getSize();
            $fileRecord = array(
                'fileName' => $fileName,
                'fileSize' => $fileSize,
                'path' => 'videos/'.$fileName,
            );
        }
    } catch (\InvalidArgumentException $e) { }
  
    return new FileField($fileRecord);
}
```

## Step 4. Add the call to extractVideo method in method fillCatalogElementDataMap

Method fillCatalogElementDataMap is responsible for generating the data map of the catalog element.

Add the call to extractVideo method in method fillCatalogElementDataMap

``` php
/**
 * Fills the data map of a catalog element
 *
 * @param CatalogElement $catalogElement
 * @param array $dataMap
 * @return CatalogElement
 */
protected function fillCatalogElementDataMap(CatalogElement $catalogElement, array $dataMap = array())
{
.
.
.
  
    /* Before returning the catalog element we can add the video to data map: */
  
    // The videos will be only for products, so:
    if ($catalogElement instanceof ProductNode) {
        $videoField = $this->extractVideo('ses_sku', $dataMap);
        $catalogElement->addFieldToDataMap('video', $videoField);
    }
 
    return $catalogElement;
}
```

Done!

Your catalog element now should have a video file in its data map.

Keep in mind the following: data map is an array with different kinds of field objects which are defined here: EshopBundle/Content/Fields

Also arrays of fields can be added. In this case the object will be an ArrayField, and its contents will be Field Objects. You can have an ArrayField with different files fields, text fields or any other kind of defined field.
