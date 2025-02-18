# rollun-ts-datastore
Basically [rollun-datastore](https://github.com/rollun-com/rollun-datastore),
but in TS. For now contains only HTTP Datastore.

## Installation
preferred way to install this library is via npm. Run
```
npm install rollun-ts-datastore
```
or add
```
"rollun-ts-datastore": "*",
```
to the dependencies section of your package.json
## Basic usage
```js
import HttpDatastore from "rollun-ts-datastore/dist/HttpDatastore";
import Query from "rollun-ts-rql/dist/Query";
import And from "rollun-ts-rql/dist/nodes/logicalNodes/And";
import Le from "rollun-ts-rql/dist/nodes/scalarNodes/Le";
import Ge from "rollun-ts-rql/dist/nodes/scalarNodes/Ge";

// keep in mind, using HttpDatastore server side (node.js), 
// You MUST spesify absolute path to your api
const datastore = new HttpDatastore('my/users/datastore');

const newItem = {id: '123', name: 'Alex', age: 23};
//add item to store
datastore.create(newItem).then((item) => {
    console.log(item);
    // will log newly created item
    // useful when using autogenerated IDs
});

//read item from store
datastore.read('123').then(
    (item) => {
        // do something with item
        // ...
    },
    (error) => {
        console.warn('an error occurred while reading from datastore', error)
    },
);

//update item (by id)
datastore.update({id: '123', age: 24}).then((item) => {
    console.log(item);
    // will log updated item;
});

//delete item
datastore.delete('123').then((item) => {
    console.log(item);
    // will log deleted item;
});

//get items for query
const query = new Query({
    query: new And([
        new Le('age', 25),
        new Ge('age', 18),
    ])
});

datastore.query(query).then(
    (items) => {
        //do something with result
        //...
    },
    (error) => {
        console.warn(error)
    }
);
```
