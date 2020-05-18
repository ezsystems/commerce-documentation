# Customer SKUs

In some projects (B2B) customers know products by their own article number.
This means that there is the 'real' article number (SKU), that comes from the ERP,
but when the customer e.g. searches for a product in the shop, they search for an article number that is only known to them.

The shop often knows these customer article numbers, because it imports them from ERP.
They are taken into account in search and quick order management.

You can activate/deactivate `CustomerSkuService` using the `customer_sku_service_active` configuration parameter:

``` yaml
#values: true or false
siso_core.default.customer_sku_service_active: true
```

## CustomerSkuService

Custom SKUs are handled using the `Silversolutions\Bundle\EshopBundle\Service\CustomerSkuService` service
(ID: `siso_core.customer_sku_service`).

`CustomerSkuService` is used to fetch the `sku` or `customer_sku`. For that purpose the following methods are available:

``` php
public function getSku($customerSku, $customerNumber = null)
public function getCustomerSku($sku, $customerNumber)
public function getOneByCustomerSku($customerSku, $customerNumber = null)
public function getOneBySku($sku, $customerNumber)
```

### Search changes

`Siso/Bundle/SearchBundle/EventListener/CustomerSkuEshopQueryListener` in the search bundle
checks the search term and if it is a customer SKU, it replaces it before sending it to Solr.

### Quick order changes

Quick order supports using the customer SKU for all functionalities:

- updating the quick order
- adding to basket
- uploading CSV
- inputting CSV content

Before the SKU is processed, it is checked for customer SKU and replaced if this is the case.

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

## Twig functions

Two Twig functions can output SKUs in a template:

``` 
{{ get_sku('customer_sku', 'customer_number') }}
{{ get_customer_sku('real_sku', ses.profile.sesUser.customerNumber) }}
```
