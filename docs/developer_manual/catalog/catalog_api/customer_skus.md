# Customer SKUs

Advanced version only

## Introduction

In some projects (B2B) customers know products by their own article number. That means, that there is the 'real' article number (sku), that comes from the ERP, but when the customer e.g. searches for a product in the shop, he searches for an article number, that is only known to him.

In the shop we often know these customer article numbers, because we import them from ERP, but we have to consider them in shop in several places:

|Considered|Not Considered (yet)|
|--- |--- |
|Search</br>Quickorder|Autosuggest|

!!! note "Important"

    It is possible to activate/deactivate the service if necessary by setting the parameter to the desired value:

    `silver.eshop.yml`:

    ``` yaml
    #here you can enable/disable the CustomerSkuService
    #values: true or false
    siso_core.default.customer_sku_service_active: true
    ```

### Search changes

An event listener was implemented in the search bundle: `Siso/Bundle/SearchBundle/EventListener/CustomerSkuEshopQueryListener.php`

This event listener checks the search term and if it is a customer sku, it exchanges it before send to solr.

### Quickorder changes

Also in the quickorder the customer sku is supported for all functionalities:

- update quickorder
- add to basket
- upload csv
- input csv content

Before the sku is processed, it is checked for customer sku and exchanged if this is the case.

## Customer SKU table

The customer SKUs are stored in a special table together with the corresponding customer numbers (see `silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Entity/CustomerSku.php`):

`ses_customer_sku`:

``` 
+-----------------+--------------+------+-----+---------+----------------+
| Field           | Type         | Null | Key | Default | Extra          |
+-----------------+--------------+------+-----+---------+----------------+
| id              | int(11)      | NO   | PRI | NULL    | auto_increment |
| sku             | varchar(255) | NO   |     | NULL    |                |
| customer_sku    | varchar(255) | NO   |     | NULL    |                |
| customer_number | varchar(255) | YES  |     | NULL    |                |
| notice          | varchar(255) | YES  |     | NULL    |                |
+-----------------+--------------+------+-----+---------+----------------+ 
 
+----+--------+----------------+-----------------+---------------+
| id | sku    | customer_sku   | customer_number | notice        |
+----+--------+----------------+-----------------+---------------+
|  1 | D4142  | 23456          | D00210          | ThisIsANotice |
|  2 | D5502  | 12345          | D00210          | ThisIsANotice |
+----+--------+----------------+-----------------+---------------+
Indexes in ses_customer_sku:
+------------------+------------+---------------------+--------------+-----------------+-----------+-------------+----------+--------+------+------------+---------+---------------+
| Table            | Non_unique | Key_name            | Seq_in_index | Column_name     | Collation | Cardinality | Sub_part | Packed | Null | Index_type | Comment | Index_comment |
+------------------+------------+---------------------+--------------+-----------------+-----------+-------------+----------+--------+------+------------+---------+---------------+
| ses_customer_sku |          0 | PRIMARY             |            1 | id              | A         |      305518 |     NULL | NULL   |      | BTREE      |         |               |
| ses_customer_sku |          1 | ind_customer_sku    |            1 | customer_sku    | A         |      305518 |     NULL | NULL   |      | BTREE      |         |               |
| ses_customer_sku |          1 | ind_customer_number |            1 | customer_number | A         |      305518 |     NULL | NULL   | YES  | BTREE      |         |               |
| ses_customer_sku |          1 | ind_sku             |            1 | sku             | A         |      101839 |     NULL | NULL   |      | BTREE      |         |               |
+------------------+------------+---------------------+--------------+-----------------+-----------+-------------+----------+--------+------+------------+---------+---------------+
```

## CustomerSkuService

``` 
id: siso_core.customer_sku_service
namespace: Silversolutions\Bundle\EshopBundle\Service\CustomerSkuService;
```

The CustomerSkuService is used to fetch the sku or customer_sku. For that purpose two methods are available:

Methods:

``` php
public function getSku($customerSku, $customerNumber = null)
public function getCustomerSku($sku, $customerNumber)
public function getOneByCustomerSku($customerSku, $customerNumber = null)
public function getOneBySku($sku, $customerNumber)
```

## Twig methods

Two twig functions have been added, they can be used to output SKUs in a template

`SilvercommonExtension.php`:

``` 
new \Twig_SimpleFunction('get_customer_sku',                array($this, 'getCustomerSku')),
new \Twig_SimpleFunction('get_sku',                         array($this, 'getSku')),
```

Usage in templates:

``` 
{{ get_sku('customer_sku', 'customer_number') }}
{{ get_customer_sku('real_sku', ses.profile.sesUser.customerNumber) }}
```
