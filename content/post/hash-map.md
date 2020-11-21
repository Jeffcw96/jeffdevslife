+++
author = "Jeff Chang"
title = "HashMap"
date = "2020-11-21"
description = "Javascript itself comes with Object and Hashmaps(Map) that will organize and arrange the data into key-value pairs. Hashmap are organized as linked list in the sense that it remembers the original insertion order of the keys"
tags = [
    "javascript","algorithm & data-structure"
]
metakeywords = "Javascript, Hash Map, Map, Hash Table, Algorithm and Data Structure"
+++

Have you ever encountering the issue of long processing time by iterating an array using nested for loop? If yes, **Hashmap(Map)** might be a good alternative choice to achieve the better result. Javascript itself comes with Hashmaps data structure that can organize and arrange the data into key-value pairs. They are organized as linked list in the sense that it remembers the original insertion order of the keys
<!--more-->

First, they are several useful methods and properties introduced from this data structure:

* HashMap.size returns the number of elements in the hashmap.
* HashMap.clear() removes all element in the hashmap.
* HashMap.set( <**key**> , <**value**> ) set and store the value for the key in the hashmap
* HashMap.get( <**key**> ) returns the value for this particular key.
* HashMap.has( <**key**> ) returns a boolean. 
    <li style="list-style:none">- <strong>True</strong> = Key is exisitng in the hashmap</li>
    <li style="list-style:none">- <strong>False</strong> = Key is Not exisitng in the hashmap</li>
* HashMap.delete( <**key**> ) returns a boolean.
    <li style="list-style:none">- <strong>True</strong> = the existed object has been successfully removed</li>
    <li style="list-style:none">- <strong>False</strong> = element is not found </li> 


### Set:
{{< highlight js >}}
let user = new Map();
user.set("name","Jeff");
user.set("age",24);
user.set("isAlive",true);
{{< /highlight >}}

Result In Browser

![hashmap example](/images/map_1.JPG)
<small style="display:block">Previous value will be overwritten by inserting the new value with same key</small>
### Get:
{{< highlight js >}}
user.set("name"); // return "Jeff"
user.set("age"); // return 24
user.set("isAlive"; // return true
{{< /highlight >}}

### Has:
{{< highlight js >}}
user.has("name"); // return true
user.has("hobby"); // return false
{{< /highlight >}}

### Delete:
{{< highlight js >}}
user.delete("isAlive"); // return true
user.has("hobby"); // return false
{{< /highlight >}}
