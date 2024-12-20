---
date: '2023-10-05T18:41:32+01:00'
draft: false
title: 'Magento store selection in a bootstrap script'
slug: 'magento-store-selection-in-bootstrap-script'
---

```php
<?php

$sm = $di->get(\Magento\Store\Model\StoreManagerInterface::class);
$storeId = null;
$storeIds = [];

foreach($sm->getStores(true, true) as $store) {
    $storeIds[] = $store->getId();
    echo "{$store->getId()} - {$store->getName()}\n";
}

while(!in_array($storeId, $storeIds)){
    echo "Choose store ID: ";
    $handle = fopen("php://stdin","r");
    $storeId = trim(fgets($handle));
    fclose($handle);
    if(!in_array($storeId, $storeIds)) {
        echo "Error! Invalid store ID.\n";
    }
}

$sm->setCurrentStore((int)$storeId);
echo "Current store was set to {$storeId}.";
```
