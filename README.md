A collection of filter functions.

#Default transforms
  - <a href="#sort">sort</a>
  - <a href="#replace">replace</a>
  - <a href="#custom">custom</a>
  - <a href="#split">split</a>
  - <a href="#filter">filter</a>
  - <a href="#merge">merge</a>
  - <a href="#remove">remove</a>
  - <a href="#removeProp">removeProp</a>
  - <a href="#toInt">toInt </a>
  - <a href="#toFloat">toFloat</a>
  - <a href="#renameCollection">renameCollection</a>
  - <a href="#renameProperty">renameProperty </a>

--------------------------------------------------------------
#Documentation

####<a name="split"></a>**split**(*option*)

Split one string property into multiple properties according to the separator.

#####Arguments
  - option (Object): The configuration of this transform. It should contain following properties:

    - *collection* (String):        The collection being modified. Optional if current collection has been set previously via *setCurrentCollection()*
    - *property*   (String):        The property whose value is being splited. The value being splited must be a string.
    - *separator*  (String|RegExp): The character, or regular expression, to use for splitting the string.
    - *names*:     (Array[String]): Optional. An array of strings that specifies the names of new properties generated by the split. If not provided, they will be generated automatically by appending 0, 1, 2, ... to the end of *property* specified above.
    
#####Returns
  - The *this* binding of the KimFilter object being applied.
  
#####Example
    
```javascript
var data = {
  "name": "sample_input",
  "results": {
    "collection1": [
      { "ID": "1.", "Karma": "329 points" },
      { "ID": "2.", "Karma": "171 points" },
      { "ID": "3.", "Karma": "145 points" }
    ]
  }
};

new KimFilter(data)
.setCurrentCollection("collection1")
.split({
  property: "Karma",
  separator: " ",
  names: ["Num", "Unit"]
})
.output(function(data) {
  console.log(data);
 });

// will print
// {
//   "name": "sample_input",
//   "results": {
//     "collection1": [
//       { "ID": "1.", "Num": "329", "Unit": "points" },
//       { "ID": "2.", "Num": "171", "Unit": "points" },
//       { "ID": "3.", "Num": "145", "Unit": "points" }
//     ]
//   }
// };
```

--------------------------------------------------------------

####<a name="sort">***sort**(*option*)</a>

Sort a collection according to the specified property.

#####Arguments
  - option (Object): The configuration of this transform. It should contain following properties:

    - *collection* (String):        The collection being modified. Optional if current collection has been set previously via *setCurrentCollection()*
    - *property*   (String):        The property being compared during sort.
    - *lowToHigh*  (boolean):       Set to 1 to sort in ascending order, 0 otherwise.
    
#####Returns
  - The *this* binding of the KimFilter object being applied.
  
#####Example
    
```javascript
var data = {
  "name": "sample_input",
  "results": {
    "collection1": [
      { "ID": "1.", "Karma": "329 points" },
      { "ID": "2.", "Karma": "171 points" },
      { "ID": "3.", "Karma": "145 points" }
    ]
  }
};

new KimFilter(data)
.setCurrentCollection("collection1")
.split({
  property: "Karma",
  lowToHigh: 1
})
.output(function(data) {
  console.log(data);
 });

// will print
// {
//   "name": "sample_input",
//   "results": {
//     "collection1": [
//       { "ID": "3.", "Num": "145", "Unit": "points" }
//       { "ID": "2.", "Num": "171", "Unit": "points" },
//       { "ID": "1.", "Num": "329", "Unit": "points" },
//     ]
//   }
// };
```


--------------------------------------------------------------

####<a name="remove">**remove**(*option*)</a>

Remove items in a collection that does not satisfy given conditions

#####Arguments
  - option (Object): The configuration of this transform. It should contain following properties:

    - *collection* (String):        The collection being modified. Optional if current collection has been set previously via *setCurrentCollection()*
    - *property*   (String):        The property being tested.
    - *opreator*   (String):        The operator being used when comparing property against given target. The operator must be one of <, <=, >, >=, ==, ===, !=, !==.
    - *target*     (String|Number|Undefined): The target value being compared against when testing.
    
#####Returns
  - The *this* binding of the KimFilter object being applied.
  
#####Example
    
```javascript
var data = {
  "name": "sample_input",
  "results": {
    "collection1": [
      { "ID": "1", "Karma": "329 points" },
      { "ID": "2", "Karma": "171 points" },
      { "ID": "3", "Karma": "145 points" }
      { "ID": "4", "Karma": "49 points" },
      { "ID": "5", "Karma": "129 points" }
    ]
  }
};

new KimFilter(data)
.setCurrentCollection("collection1")
.remove({
  property: 'ID',
  operator: '<=',
  target: 2
})
.output(function(data) {
  console.log(data);
 });

// will print
// {
//   "name": "sample_input",
//   "results": {
//     "collection1": [
//       { "ID": "1.", "Num": "329", "Unit": "points" },
//       { "ID": "2.", "Num": "171", "Unit": "points" },
//     ]
//   }
// };
```


--------------------------------------------------------------

####<a name="merge">**merge**(*option*)</a>

Merge mutlitple properties into one property.

#####Arguments
  - option (Object): The configuration of this transform. It should contain following properties:

    - *collection*    (String):        The collection being modified. Optional if current collection has been set previously via *setCurrentCollection()*
    - *properties*    (Array[String]): An array of properties being merged.
    - *newProp*       (String):        The name of merged property.
    - *newProperties* (Array[String]): An array of new names of properties being merged. Old names will be used if new names are not provided.
    
#####Returns
  - The *this* binding of the KimFilter object being applied.
  
#####Example
    
```javascript
var data = {
  "name": "sample_input",
  "results": {
    "collection1": [
      { "ID": "1", "Karma": "329 points", "href": "https://abc.com" },
      { "ID": "2", "Karma": "171 points", "href": "https://def.com" },
      { "ID": "3", "Karma": "145 points", "href": "https://hij.com" },
      { "ID": "4", "Karma": "49 points", "href": "https://klm.com" },
      { "ID": "5", "Karma": "129 points", "href": "https://nop.com" }
    ]
  }
};

new KimFilter(data)
.setCurrentCollection("collection1")
.merge({
  properties: ['Karma', 'href'],
  newProp: 'Data',
  newProperies: ['data_karma', 'data_href']
})
.output(function(data) {
  console.log(data);
 });

// will print
// {
//   "name": "sample_input",
//   "results": {
//     "collection1": [
//       { "ID": "1", Data: { "data_karma": "329 points", "data_href": "https://abc.com" }},
//       { "ID": "2", Data: { "data_karma": "171 points", "data_href": "https://def.com" }},
//       { "ID": "3", Data: { "data_karma": "145 points", "data_href": "https://hij.com" }},
//       { "ID": "4", Data: { "data_karma": "49 points", "data_href": "https://klm.com" }},
//       { "ID": "5", Data: { "data_karma": "129 points", "data_href": "https://nop.com" }}
//     ]
//   }
// }
```


--------------------------------------------------------------

####**removeProp**(*option*)

Remove one or more properteis from collection.

#####Arguments
  - option (Object): The configuration of this transform. It should contain following properties:

    - *collection*    (String):        The collection being modified. Optional if current collection has been set previously via *setCurrentCollection()*
    - *properties*    (Array[String]): An array of properties being removed.
    
#####Returns
  - The *this* binding of the KimFilter object being applied.
  
#####Example
    
```javascript
var data = {
  "name": "sample_input",
  "results": {
    "collection1": [
      { "ID": "1", "Karma": "329 points", "href": "https://abc.com" },
      { "ID": "2", "Karma": "171 points", "href": "https://def.com" },
      { "ID": "3", "Karma": "145 points", "href": "https://hij.com" },
      { "ID": "4", "Karma": "49 points", "href": "https://klm.com" },
      { "ID": "5", "Karma": "129 points", "href": "https://nop.com" }
    ]
  }
};

new KimFilter(data)
.setCurrentCollection("collection1")
.remove({
  properties: ['Karma', 'href']
})
.output(function(data) {
  console.log(data);
 });

// will print
// {
//   "name": "sample_input",
//   "results": {
//     "collection1": [
//       { "ID": "1" },
//       { "ID": "2" },
//       { "ID": "3" },
//       { "ID": "4" },
//       { "ID": "5" }
//     ]
//   }
// };
```


--------------------------------------------------------------

####**renameProperty**(*option*)

Rename one properties.

#####Arguments
  - option (Object): The configuration of this transform. It should contain following properties:

    - *collection* (String): The collection being modified. Optional if current collection has been set previously via *setCurrentCollection()*.
    - *property*   (String): The property being renamed.
    - *newname*    (String): The new name to be used.


#####Returns
  - The *this* binding of the KimFilter object being applied.
  
#####Example
    
```javascript
var data = {
  "name": "sample_input",
  "results": {
    "collection1": [
      { "ID": "1", "Karma": "329 points", "href": "https://abc.com" },
      { "ID": "2", "Karma": "171 points", "href": "https://def.com" },
      { "ID": "3", "Karma": "145 points", "href": "https://hij.com" },
      { "ID": "4", "Karma": "49 points", "href": "https://klm.com" },
      { "ID": "5", "Karma": "129 points", "href": "https://nop.com" }
    ]
  }
};

new KimFilter(data)
.setCurrentCollection("collection1")
.renameProperty({
  property: 'href',
  newname: 'link'
})
.output(function(data) {
  console.log(data);
 });

// // will print
// var data = {
//   "name": "sample_input",
//   "results": {
//     "collection1": [
//       { "ID": "1", "Karma": "329 points", "link": "https://abc.com" },
//       { "ID": "2", "Karma": "171 points", "link": "https://def.com" },
//       { "ID": "3", "Karma": "145 points", "link": "https://hij.com" },
//       { "ID": "4", "Karma": "49 points", "link": "https://klm.com" },
//       { "ID": "5", "Karma": "129 points", "link": "https://nop.com" }
//     ]
//   }
// };
```


--------------------------------------------------------------

####**renameCollection**(*option*)

Rename one collection.

#####Arguments
  - option (Object): The configuration of this transform. It should contain following properties:

    - *collection* (String): The collection being named. Optional if current collection has been set previously via *setCurrentCollection()*.
    - *newname*    (String): The new name to be used.

    Note: if the collection being renamed is the current collection(i.e. via a previous call to *setCurrentCollection()*), then current collection will be
    renamed automatically.

  
#####Returns
  - The *this* binding of the KimFilter object being applied.

#####Example
    
```javascript
var data = {
  "name": "sample_input",
  "results": {
    "collection1": [
      { "ID": "1", "Karma": "329 points", "href": "https://abc.com" },
      { "ID": "2", "Karma": "171 points", "href": "https://def.com" },
      { "ID": "3", "Karma": "145 points", "href": "https://hij.com" },
      { "ID": "4", "Karma": "49 points", "href": "https://klm.com" },
      { "ID": "5", "Karma": "129 points", "href": "https://nop.com" }
    ]
  }
};

new KimFilter(data)
.setCurrentCollection("collection1")
.renameCollection({
  newname: 'HackerNews'    
})
.output(function(data) {
  console.log(data);
 });

// // will print
// var data = {
//   "name": "sample_input",
//   "results": {
//     "HackerNews": [
//       { "ID": "1", "Karma": "329 points", "href": "https://abc.com" },
//       { "ID": "2", "Karma": "171 points", "href": "https://def.com" },
//       { "ID": "3", "Karma": "145 points", "href": "https://hij.com" },
//       { "ID": "4", "Karma": "49 points", "href": "https://klm.com" },
//       { "ID": "5", "Karma": "129 points", "href": "https://nop.com" }
//     ]
//   }
// };
```


--------------------------------------------------------------

####**replace**(*option*)

The replace method searches a string for a specified value, or a regular expression, and replace the matched pattern with the given string.

#####Arguments
  - option (Object): The configuration of this transform. It should contain following properties:

    - *collection* (String): The collection being named. Optional if current collection has been set previously via *setCurrentCollection()*.
    - *property*   (String): The property whose value is being searched and modified.
    - *from*       (String|RegExp): The value, or regular expression, that will be replaced by the new value.
    - *to*         (String|RegExp): The value to replace the *from* with.

#####Returns
  - The *this* binding of the KimFilter object being applied.

#####Example
    
```javascript
var data = {
  "name": "sample_input",
  "results": {
    "collection1": [
      { "ID": "1", "Karma": "329 points", "href": "https://abc.com" },
      { "ID": "2", "Karma": "171 points", "href": "https://def.com" },
      { "ID": "3", "Karma": "145 points", "href": "https://hij.com" },
      { "ID": "4", "Karma": "49 points", "href": "https://klm.com" },
      { "ID": "5", "Karma": "129 points", "href": "https://nop.com" }
    ]
  }
};

new KimFilter(data)
.setCurrentCollection("collection1")
.replace({
  property: 'href',
  from: 'https',
  to: 'http'
})
.output(function(data) {
  console.log(data);
 });

// // will print
// var data = {
//   "name": "sample_input",
//   "results": {
//     "HackerNews": [
//       { "ID": "1", "Karma": "329 points", "href": "http://abc.com" },
//       { "ID": "2", "Karma": "171 points", "href": "http://def.com" },
//       { "ID": "3", "Karma": "145 points", "href": "http://hij.com" },
//       { "ID": "4", "Karma": "49 points", "href": "http://klm.com" },
//       { "ID": "5", "Karma": "129 points", "href": "http://nop.com" }
//     ]
//   }
// };
```


--------------------------------------------------------------

####**toInt**(*option*)

Convert string to integer.

#####Arguments
  - option (Object): The configuration of this transform. It should contain following properties:

    - *collection* (String): The collection being named. Optional if current collection has been set previously via *setCurrentCollection()*.
    - *property*   (String): The property whose value is being converted.

#####Returns
  - The *this* binding of the KimFilter object being applied.

#####Example
    
```javascript
var data = {
  "name": "sample_input",
  "results": {
    "collection1": [
      { "ID": "1", "Karma": "329 points", "href": "https://abc.com" },
      { "ID": "2", "Karma": "171 points", "href": "https://def.com" },
      { "ID": "3", "Karma": "145 points", "href": "https://hij.com" },
      { "ID": "4", "Karma": "49 points", "href": "https://klm.com" },
      { "ID": "5", "Karma": "129 points", "href": "https://nop.com" }
    ]
  }
};

new KimFilter(data)
.setCurrentCollection("collection1")
.toInt({
  property: 'ID'
})
.output(function(data) {
  console.log(data);
 });

// // will print
// var data = {
//   "name": "sample_input",
//   "results": {
//     "HackerNews": [
//       { "ID": 1, "Karma": "329 points", "href": "https://abc.com" },
//       { "ID": 2, "Karma": "171 points", "href": "https://def.com" },
//       { "ID": 3, "Karma": "145 points", "href": "https://hij.com" },
//       { "ID": 4, "Karma": "49 points", "href": "https://klm.com" },
//       { "ID": 5, "Karma": "129 points", "href": "https://nop.com" }
//     ]
//   }
// };
```


--------------------------------------------------------------

####**toFloat**(*option*)

Convert string to floating point number.

#####Arguments
  - option (Object): The configuration of this transform. It should contain following properties:

    - *collection* (String): The collection being named. Optional if current collection has been set previously via *setCurrentCollection()*.
    - *property*   (String): The property whose value is being converted.
    - *decimal*    (String): The number of digits to appear after the decimal point; this may be a value between 0 and 20, inclusive, and implementations may optionally support a larger range of values. If this argument is omitted, it is treated as 0.

#####Returns
  - The *this* binding of the KimFilter object being applied.

#####Example
    
```javascript
var data = {
  "name": "sample_input",
  "results": {
    "collection1": [
      { "ID": "1", "Karma": "329.21331", "href": "https://abc.com" },
      { "ID": "2", "Karma": "171.321", "href": "https://def.com" },
      { "ID": "3", "Karma": "145.943", "href": "https://hij.com" },
      { "ID": "4", "Karma": "49.123213", "href": "https://klm.com" },
      { "ID": "5", "Karma": "129.9", "href": "https://nop.com" }
    ]
  }
};

new KimFilter(data)
.setCurrentCollection("collection1")
.toFloat({
  property: 'Karma',
  decimal: 3
})
.output(function(data) {
  console.log(data);
 });

// // will print
// var data = {
//   "name": "sample_input",
//   "results": {
//     "HackerNews": [
//       { "ID": "1", "Karma": 329.213, "href": "https://abc.com" },
//       { "ID": "2", "Karma": 171.321, "href": "https://def.com" },
//       { "ID": "3", "Karma": 145.943, "href": "https://hij.com" },
//       { "ID": "4", "Karma": 49.123, "href": "https://klm.com" },
//       { "ID": "5", "Karma": 129.900, "href": "https://nop.com" }
//     ]
//   }
// };
```


--------------------------------------------------------------

####**custom**(*fn*)

Apply user-defined transform functions.

#####Arguments
  - fn (Function): The user-defined transform function being applied. The *data* object will be passed to this function.

#####Returns
  - The *this* binding of the KimFilter object being applied.

#####Example
    
```javascript
var data = {
  "name": "sample_input",
  "results": {
    "collection1": [
      { "ID": "1", "Karma": "329.21331", "href": "https://abc.com" },
      { "ID": "2", "Karma": "171.321", "href": "https://def.com" },
      { "ID": "3", "Karma": "145.943", "href": "https://hij.com" },
      { "ID": "4", "Karma": "49.123213", "href": "https://klm.com" },
      { "ID": "5", "Karma": "129.9", "href": "https://nop.com" }
    ]
  }
};

new KimFilter(data)
.custom(function(data) {
  var collection = 'HackerNews';
  data[collection].forEach(function(entry, idx, arr) {
    entry['timestamp'] = (new Date()).toUTCString();
  });
})
.output(function(data) {
  console.log(data);
 });

// // will print
// var data = {
//   "name": "sample_input",
//   "results": {
//     "HackerNews": [
//       { "ID": "1", "Karma": 329.213, "href": "https://abc.com", "timestamp": "Mon, 12, Jan 2015 3:14:15 GMT" },
//       { "ID": "2", "Karma": 171.321, "href": "https://def.com", "timestamp": "Mon, 12, Jan 2015 3:14:15 GMT" },
//       { "ID": "3", "Karma": 145.943, "href": "https://hij.com", "timestamp": "Mon, 12, Jan 2015 3:14:15 GMT" },
//       { "ID": "4", "Karma": 49.123, "href": "https://klm.com", "timestamp": "Mon, 12, Jan 2015 3:14:15 GMT" },
//       { "ID": "5", "Karma": 129.900, "href": "https://nop.com", "timestamp": "Mon, 12, Jan 2015 3:14:15 GMT" }
//     ]
//   }
// };
```
