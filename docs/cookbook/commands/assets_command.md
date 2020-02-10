#  Assets Command 

This command is used to create cache for assets, using current data provider.

# Usage

``` 
bin/console silversolutions:cache-asset:generate
```

## Workflow

1.  Command collects all products.
2.  Loop through those products and recreate the asset cache for it
