# *Big Map* for node js

----------------------


This package provides you with a big map.   
It uses Buffer as its storage space rather than heap memory.  
It implemented by node js pure javascript code (ES6), no other dependencies.     
Its feature is still relatively simple, and performance worse than build-in Object or Map,   
but sometimes in order to get more space, we have no other choice.

Use Buffer implementation does not exist v8 engine heap memory limit of 1.4GB,   
you can store 4GB, 8GB or more data therein, as long as sufficient physical memory.

### When use 

* heap memory will not meet your demand  
* the keys and values have consistent data type   
* you know the max length of your key and value   

### When NOT use

* performance sensitive
* can not determine the data type
* can not determine the max length of key or value  
* you need remove/delete entries


------------------------

### Usage

##### install :
```
npm install big-map
```
##### quick start :
```javascript
var BigMap = require('big-map');

var bigmap = new BigMap(16, 16);

bigmap.set('hello', 'world'); 
// -> true

bigmap.get('hello')
// -> 'world'
```

------------------------------


### new BigMap(key_length, value_length\[, options\])

construct a new BigMap

* `key_length` : required. specify the max key string length. recommend a multiple of 4.
* `value_length` : required. specify the max value string length. recommend a multiple of 4.
* `options` : optional.
    * `keyType` : only support string now.
    * `valueType` : support string and number. default is string. note: if value type set to number, the value_length will be override by 8 (DoubleFloat).
    * `loadFactor` : default is 0.75
    * `migrate` : whether rehash data from old buffer to new bigger buffer. default is false
    * `async_migrate` : if migrate, use async mode or sync mode. default is false


### set(key, value)

set key value pair to big map.
   
you should make sure the key and value has valid data type and length,   
or else it will throw a error. 

* `key` : your string key
* `value` : your value, default it needs string type. you can pass a number if you set valueType options to number. 

`return` boolean, true if it success.  

### get(key)

get a value by the key.

* `key` : your string key

`return` string or number, depends on the options. if not find the key, return undefined.

### properties

* `.id` : {string} BigMap id
* `.size` : {number} the number of the elements have been set.
* `.migrating` : {number} 0 if not in migrating status, or else, it has N MapBlocks in migrating status.

### TODO

* add more test case, make sure basic functionality
* key support number type
* add remove function
* change data structure, add a meta byte