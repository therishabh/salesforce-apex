# Salesforce development

- Programming world:
+
=
<=
{
)
Integer
String
Class
Trigger
If
Else 
etc ....



## Collections
- There are 3 types of collections.


### List Collection
(https://intellipaat.com/blog/collection-in-salesforce/#no3)

A list can hold any type of data and is one of the most important types of collection. A list is an ordered collection of any data type such as primitive types, collections, sObjects, user-defined types, and built-in Apex types.

The following are the key features of a list collection in Salesforce:

- Duplicates and nulls are allowed in a list collection.
- "List" is the keyword to declare a list collection.
- In a list collection, the index position of the first entry is always zero (0).
- An Apex list collection has the ability to grow dynamically over time.
- In a list collection, the data type can be both primitive and non-primitive.

The keyword followed by the data type has to be used within <> characters to define a list collection.

The syntax for a list is as follows:
```apex
// Create an empty list of String
List<String> my_list = new List<String>();
```

```apex
List<Integer> myList = new List<Integer>();   // Define a new list
myList.add(47);                               // Adds a second element of value 47 to the end of the list
Integer i = myList.get(0);                    // Retrieves the element at index 0
myList.set(0, 1);                             // Adds the integer 1 to the list at index 0
myList.remove(0)                              // Remove element of 0 index
myList.clear();                               // Removes all elements from the list
integer myListLength = myList.size();         // How many items in the list
```

```apex
List<String> myList = new List<String>();
myList.add('Harry');
myList.add('Ram');
myList.add('Shyam');
myList.add('Rishabh');
myList.add('Madhav');
myList.add('Shikha');
system.debug('Collection list : '+myList);
myList.remove(2);
myList.set(3,'Hari');

integer totalListLength = myList.size();
system.debug('Total List length : ' + totalListLength);

// check if any item exits inside list
if(myList.contains('Rishabh')){
    system.debug('Yes, Rishabh is exist');
}
if(myList.contains('Pankaj')){
    system.debug('Yes, Pankaj is exist');
} else {
  system.debug('Pankaj is not exist inside the list');          
}

// for each loop
for(string name: myList){
    system.debug('Person Name : ' + name);
}

// check if myList empty
if(myList.isEmpty()){
    system.debug('myList is empty');
} else {
    system.debug('myList is not empty and it contain '+myList.size()+' items');
}
// clear list
myList.clear();
if(myList.isEmpty()){
    system.debug('myList is empty');
}

```
**Output**
<img width="1362" alt="Screenshot 2024-04-02 at 4 52 38 PM" src="https://github.com/therishabh/salesforce-dev/assets/7955435/ab821b66-d603-4afd-8c47-e0d3c7915aa8">

- What is the difference between Add Vs Set function in List collection ?
  A. Set is used for replace any value while Add is used for add new value at the end of the list we don't need to pass index number in Add.

### Set Collection
Set is an unordered collection. It is a unique set of values that do not have any duplicates. If you want no duplicates in your collection, then you should opt for this collection.

**The following are the key features of a set collection in Salesforce:**

- Any data types, such as primitive data types and sObjects, are allowed in a set collection.
- A set collection does not have an index.
- A set collection does not contain any duplicates or null values.
- Set collections are not widely used in Salesforce Apex.
- Sets can hold collections that are nested inside of one another.
- You have to use the set keyword followed by the primitive data type name within <> characters to declare a set collection.

**The syntax for a set is as follows:**
```apex
Set<datatype> setName = new Set<datatype>();
```

### Map Collection
Map is a key-value pair that contains each value’s unique key. It is used when something needs to be located quickly.

**The following are the key features of a map collection in Salesforce:**

- Any data type can be used for both the key and the value in a map collection.
- In a map collection, the null value can be stored in a map key.
- The keys of type string are case-sensitive in a map collection.
- You have to use the map keyword followed by the key and value data types enclosed in <> to declare a map collection.

**The syntax for a map is as follows:**
```apex
Map<String, String> country_currencies = new Map<String, String>();
```
