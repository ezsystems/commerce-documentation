# Bestseller

eZ Commerce offers a feature to determine and display bestsellers. 

Bestsellers are determined based on all confirmed orders. The information how often a product was purchased is stored in Solr by a Solr plugin. The shop owner can specify from which point a product counts as a bestseller.

Bestsellers can be displayed on Landing Pages, category pages and bestseller page. 

## Enable bestsellers in shop

To enable bestsellers in the shop set the following parameter to true:

``` yaml
siso_core.default.enable_bestsellers: true
```

## Configure bestsellers in Back Office

Inthe Back Office, go to eZ Commerce->Configuration Settings->Miscellaneous

The following settings are available:

| Configuration        | Description      |
| -------------------- | ---------------- |
| Number of bestsellers displayed on bestseller page | Integer, how many bestsellers should be displayed on the bestsellers page          |
| Number of bestsellers displayed on catalog pages   | Integer, how many bestsellers should be displayed on category pages of the catalog |
| Number of bestsellers displayed in a slider        | Integer, how many bestsellers should be available in a slider on landing page      |
| Threshold   | How often a product has to be sold to count as a bestseller  |
