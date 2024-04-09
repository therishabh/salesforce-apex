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

List of all methods available in List : https://developer.salesforce.com/docs/atlas.en-us.apexref.meta/apexref/apex_methods_system_list.htm#apex_System_List_methods

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

## OOPS Concept

Q. What is a Class? \
- A class is a user-defined layout or blueprint of an object that describes what a specific kind of object will look like. A class description consists of two things:
      1) Attributes or member variables
      2) Implementations of behavior or member functions.
- So in object-oriented terminology: A class is a blueprint that defines the variables and the methods common to all objects of a certain kind. It helps us to bind data and methods together, making the code reusable, unlike procedural language.

Q. What is an Object? \
- An object is a single instance of a class, which contains data and methods working on that data. So an object consists of three things:

**Special Notes**
- When a class is defined, no memory is allocated, but memory is allocated when an object is created.
- We can create many objects from the same class type.


### Important points related to functions

- Functions where they will have return value = non void functions
- Functions where they will NOT have return value = void functions
- Functions where they will expect some parameters/data from us = functions with parameters
- Functions where they will NOT expect any parameters from us = functions WITHOUT parameters

Q. Why we need class in salesforce programming ?

Q. Why we need object when we already have class ?

Q. Can we use standard and custom function in programming ?

Q. Why we need void keyword in function ?

Q. What else I can write if I don't want to use void keyword ?

Q. Can I keep the blank option instead of void I don't want to raise anything so is that possible ?

Q. What is function parameter and why we need that function parameter why we need that ?

Q. I don't want to use function parameter can I keep it blank ?


**Current Moral:** \
Class = Variables + Functions

Variables are for Data storage.

Functions are for performing some Activity | Action.

Problem: Class wont have any memory, so we cant do anything!

Solution : **Object**

**Very Important Current Moral:** \
- Class = Variables + Functions (No Memory)
- Object = Variables + Functions + MEMORY
- Integer, Double, String, Boolean = primitive data type AKA system defined data type
- collection or apex class or object = user defined data type
- Whenever is user defined datatype then new keyword is required.

### Static Keyword

**See this example, how to bypass Object with the help of static keyword:**

```apex
public class AppleMobileClass (

    //variable declaration
    private static integer DurationOfCall;

    public static void CallAPerson(integer NumToCall) {
        // code to video call a person
    }
}

// Syntax to execute
ClassName.FunctionName(Parameter);

// To execute the function
AppleMobileClass.CallAPerson(9970629222);
```
SUCCESS !!! NO NEED OF OBJECT !!!

Moral 1:\
By using static, we get special/global storage.

Moral 2:\
So now by using word "static",

we can give memory to the class as well, this is the famous style of writing program in Salesforce industry. \

(which style is famous? of not using Object / instance style)

### Access Specifier

- Private
- Public
- Protect
- Global

```apex
public class APEXClassForCalculator {

    private static integer VarSum;
    private static integer VarMultiplication;
    
    // function for addition
    public static integer add(integer a, integer b){
        VarSum = a + b;
        return VarSum;
    }
    
    // function for multiplication
    public static void multiply(integer a, integer b){
        system.debug(a * b);
    }
}


APEXClassForCalculator.multiply(3,6);
```
## Trigger

#### Introduction
- Triggers are initiated when a record is inserted, updated, deleted and undeleted.
- Use triggers to do operations that cannot be done by point and click tools provided in Salesforce.
- We can do things using triggers that we can do through Apex, including execution SOQL and DML or calling custom methods.
- Triggers can be created for both Standard and Custom objects.
- We can perform custom operations before or after events to records.
- By default triggers are active as you create them.
- _Trigger class will execute automatically based on trigger event what you have definded in Trigger class._
- Trigger file extension is **apxt** while Apex class file extension is **apxc**

#### Types of Triggers
- Before Triggers
  - It is used to update or validate record values before saved to database.
- After Triggers
    - It is used to access field values that are set by the system such as Ids, and to make changes in the related/other records. The records that fire the after trigger are read-only.

#### Trigger order of Execution
<img width="875" alt="Screenshot 2024-04-09 at 12 57 35 PM" src="https://github.com/therishabh/salesforce-dev/assets/7955435/d158fd8c-25e0-4846-89d0-2a3813b33797">

#### Trigger Events
- before insert
- before update
- before delete
- after insert
- after update
- after delete
- after undelete

#### Trigger Context Variables
All triggers define implicit variables that allow developer to access run-time context. These variables are contained in the System. Trigger class.
- **isExecuting:** Returns true if the current context of Apex code is a trigger, not a VF page, a web service, or an executeanonymous) API call.
- **isinsert:** Returns true if trigger was fired due to an insert operation, from the Salesforce UI, Apex or API.
- **isUpdate:** Returns true if trigger was fired due to an update operation, from the Salesforce Ul, Apex or API.
- **isDelete:** Returns true if trigger was fired due to a delete operation, from the Salesforce Ul, Apex or API.
- **isBefore:** Returns true if the trigger was fired before any record was saved.
- **isAfter:** Returns true if the trigger was fired after all records were saved.
- **isUndelete:** Returns true if the trigger was fired after a record is recovered from Recycle Bin.
- **size:** The total number of records in a trigger invocation, both old and new
- **new:**
    - Returns a list of new versions of sObject records.
    - This sObject list is available in Insert, Update and Undelete triggers, and the records can only be modified in before trigger.
- **newMap:**
    - A Map of ids to the new versions of sObject records.
    - Available in after insert, before update, after update, after undelete triggers.
- **old:**
    - Returns a list of old versions of sObject records.
    - Available in before update, after update, before delete, after delete triggers.
- **oldMap:**
    - A map of ids to the old versions of sObject records.
    - Available in before update, after update, before delete, after delete triggers.
      
#### Trigger Context Variable Comparison 
<img width="1211" alt="Screenshot 2024-04-09 at 1 30 13 PM" src="https://github.com/therishabh/salesforce-dev/assets/7955435/29eadfad-6a82-4a8a-82a2-f80f9f3ef6b2">

#### Syntax
```apex
trigger TriggerName on ObjectName (trigger_events) {
    //code_block
}
```
#### Examples
```apex
trigger AccountTrigger on Account (before insert) {
    System.debug('Trigger before insert event');
}
```
```apex
trigger AccountTrigger on Account (before insert) {
    if(Trigger.isInsert){
        if(Trigger.isBefore){
            // Trigger.new is a sObject List
            for(Account acc: Trigger.new){
                acc.description = 'Account has been created';
            }
        }
    }
}
```
```apex
// For the best practice we need to write business logic in Apex class as Trigger Handler separately.

// Trigger
trigger AccountTrigger on Account (before insert) {
    if(Trigger.isInsert){
        if(Trigger.isBefore){
            AccountTriggerHandler.updateDescription(Trigger.new);
        }
    }
}

// Trigger Handler (Apex Class)
public class AccountTriggerHandler {
    public static void updateDescription(List<Account> accList){
        for(Account acc: accList){
            acc.Description = 'Account has been created. !!Trigger!!';
        }
    }
}
```
```apex
/*
Question :
Before Insert
    - If Account Industry is not null and having value as 'Media' then populate Rating as Hot.

After Insert
    - Create related Opportunity when Account is created.
*/

// File Name : AccountTrigger.apxt
trigger AccountTrigger on Account (before insert, after insert) {
    if(Trigger.isInsert){
        if(Trigger.isBefore){
            AccountTriggerHandler.populateRating(Trigger.new);
        } else if(Trigger.isAfter){
            // Trigger.new will be read only in After insert.
            AccountTriggerHandler.createOpportunity(Trigger.new);
        }
    }
}



// File Name : AccountTriggerHandler.apxc
public class AccountTriggerHandler {
    public static void updateDescription(List<Account> accList){
        for(Account acc: accList){
            acc.Description = 'Account has been created. !!Trigger!!';
        }
    }
    
    public static void populateRating(List<Account> accList){
        for(Account acc: accList){
            if(acc.Industry == 'Media'){
                acc.Rating = 'Hot';
            }
        }
    }
    
    public static void createOpportunity(List<Account> accList){
        List <Opportunity> oppList = new List<Opportunity>();
        
        for(Account acc: accList){
            if(acc.Active__c == 'Yes'){
                Opportunity opp = new Opportunity();
                opp.AccountId = acc.Id;
                opp.Name = acc.Name + ' Opportunity';
                opp.CloseDate = System.today();
                opp.StageName = 'Qualification';
                opp.Description = 'Opportunity has been created Through !!Account Trigger!!';
                oppList.add(opp);
            }
        }
        if(!oppList.isEmpty()){
            insert oppList;
        }
        
    }
}
```
