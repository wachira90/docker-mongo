# php connect

````
<?php
$manager = new MongoDB\Driver\Manager("mongodb://root:example@192.168.6.190:27017/");

$db = 'testdb';
$collection = 'testcol';
$db_collection = $db . '.' . $collection;

$bulk = new MongoDB\Driver\BulkWrite;
$bulk->insert(['x' => 1]);
$bulk->insert(['x' => 2]);
$bulk->insert(['x' => 3]);
$manager->executeBulkWrite($db_collection, $bulk);

$filter = ['x' => ['$gt' => 1]];
$options = [
    'projection' => ['_id' => 0],
    'sort' => ['x' => -1],
];

$query = new MongoDB\Driver\Query($filter, $options);
$cursor = $manager->executeQuery($db_collection, $query);

foreach ($cursor as $document) {
    var_dump($document);
}
````
