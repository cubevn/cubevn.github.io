# Technical Problems

1. Template : Confuse
 - New way to adding or changing content in template: Why use `ecccube_block_xxx` while you can use the code snippet?
```
eccube_block_hello({'name':'EC-CUBE'}) vs addSnippet
```


 - Listen event of Admin template:

```
Admin/Product/product.twig  -> Admin/@admin/Product/product.twig
```

2. Event :
 - Removed:
    - event: admin.order.delete.complete

3. others
    - Cannot use controller in block.
    - Unable to retrieve the total order payment amount on the shopping index page.
    - The system still charges taxes on discounts.
    - The block.twig can not get the {http_cache} (esi) parameter.
    - Difficult to inject template (  edit inline, dont use id for element - xpath ).