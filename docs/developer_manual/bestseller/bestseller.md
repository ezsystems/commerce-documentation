#  Bestseller 

## Introduction

eZ Commerce offers a feature to determine and display bestsellers. 

Bestsellers are determined based on all confirmed orders. The information how often a product was purchased is stored in Solr by a Solr-plugin. The shop owner can specify from which treshold a product counts as a bestseller.

Bestsellers can be displayed on landing pages, category pages and bestseller page. 

## Enable bestsellers in shop

Set the following parameter to true, to enable bestsellers in the shop

**silver.eshop.yml**

``` 
# Bestseller configuration
siso_core.default.enable_bestsellers: true
```

## Configure bestsellers in backend

Go to eZ Commerce→Configuration Settings→Miscellaneous

The following configurations are available

| Configuration                                                                                 | Description                                                                        |
| --------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------- |
| Number of bestsellers displayed on bestseller page | Integer, how many bestsellers should be displayed on the bestsellers page          |
| Number of bestsellers displayed on catalog pages   | Integer, how many bestsellers should be displayed on category pages og the catalog |
| Number of bestsellers displayed in a slider        | Integer, how many bestsellers should be available in a slider on landing page      |
| Treshold                                                                                      | How often has a product to be sold to count as a bestseller                        |
