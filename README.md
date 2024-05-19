# Salesforce development

### Table of Contents
1. [Collections](#collections)
    - [List Collection](#list-collection)
    - [Set Collection](#set-collection)
    - [Map Collection](#map-collection)
2. [OOPS Concept](#oops-concept)
3. [Static Keyword](#static-keyword)
4. [Access Specifier](#access-specifier)
5. [Trigger](#trigger)
6. [DML (Data Manipulation Language)](#dml-data-manipulation-language)
7. [SOQL Cheat Sheet](#soql-cheat-sheet)
8. [Asynchronous Processing Basics](#asynchronous-processing-basics)
   - [Future Method in apex](#future-method-in-apex)
   - [Batch Apex](#batch-apex)
   - [Queueable Apex](#queueable-apex)

------

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

```apex
/*
Question :
Before Update
    - If account phone is updated then put update message in description.
*/

// File Name : AccountTrigger.apxt
trigger AccountTrigger on Account (before update) {
    if(Trigger.isUpdate){
        if(Trigger.isBefore){
            AccountTriggerHandler.updatePhoneDesc(Trigger.new, Trigger.oldMap);
        }
    }
}


// File Name : AccountTriggerHandler.apxc
public class AccountTriggerHandler {
    public static void updatePhoneDesc(List<Account> accList, Map<ID, Account> accOldMap){
        //execute a loop on update account list
        for(Account acc: accList){
            if(acc.Phone != accOldMap.get(acc.Id).Phone){
                acc.Description = 'Phone number has been change. Old Value is : '+ accOldMap.get(acc.Id).Phone + ' New Value is : ' + acc.Phone;
            }
        }
    }
}
```

```apex
/*
Question:
Before Insert & Update
    - Upon Account Creation or Updation if Account Industry is not null and having value as 'Media' then populate Rating as Hot.
*/

// File Name : AccountTrigger.apxt
trigger AccountTrigger on Account (before insert, before update) {
    if(Trigger.isInsert){
        if(Trigger.isBefore){
            // we'll pass null as oldMap here because for insert field we don't have oldMap value.
            AccountTriggerHandler.populateRating(Trigger.new, null);
        }
    }

    if(Trigger.isUpdate){
        if(Trigger.isBefore){
            AccountTriggerHandler.populateRating(Trigger.new, Trigger.oldMap);
        }
    }
}


// File Name : AccountTriggerHandler.apxc
public class AccountTriggerHandler {
    public static void populateRating(List<Account> accList, Map<Id, Account> accOldMap){
        for(Account acc: accList){
            // here i am checking if this is insert operation with accOldMap == null
            // and further check for Industry value should not be null and it should be Media then execute 
            if((accOldMap == null && acc.Industry != null && acc.Industry == 'Media')){
                acc.Rating = 'Hot';
                acc.Description = 'Rating updated with Hot on Insert !!AccountTrigger!!';
            }
            // here i am checking if user touched or changed Industry field and select Media
            // with code `accOldMap.get(acc.Id).Industry` we we get old value of Industry field.
            else if(accOldMap.get(acc.Id).Industry != acc.Industry && acc.Industry == 'Media'){
                acc.Rating = 'Hot';
                acc.Description = acc.Description || '' + ' \n Rating updated with Hot on Update !!AccountTrigger!!';
            }
        }
    }
}
```

```apex
/*
Question:
Before Delete
    - Account record cannot be deleted if active is Yes.
    - Use Custom Label to show Error Message.
    - Apply a check that only System Administrator profile user can delete an account.
*/

// File Name : AccountTrigger.apxt
trigger AccountTrigger on Account (before delete) {
    if(Trigger.isBefore){
        if(Trigger.isDelete){
            AccountTriggerHandler.preventDelete(Trigger.old);
        }
    }
}


// File Name : AccountTriggerHandler.apxc
public class AccountTriggerHandler {
    public static void preventDelete(List<Account> accList){
        
        profile p = [SELECT Id FROM Profile WHERE Name = 'System Administrator'];
        
        for(Account acc: accList){
            // here userInfo.getProfileId() we will get logged-in user Id
            if(userInfo.getProfileId() != p.Id || acc.Active__c == 'Yes'){
                acc.addError(Label.Prevent_Account_Deletion );
            }
        }
    }
}

```

```apex
/*
Question:
After Delete
    - When a contact record is deleted then update 'Total Contact Count' field on related Account.
After Undelete
    - When a contact record is undeleted then update 'Total Contact Count' field on related Account.
*/

// File Name : ContactTrigger.apxt
trigger ContactTrigger on Contact (after insert, after delete, after undelete) {
    if(Trigger.isAfter){
        if(Trigger.isInsert){
            ContactTriggerHandler.updateContactCount(Trigger.new);
        }   
        
        if(Trigger.isDelete){
            ContactTriggerHandler.updateContactCount(Trigger.old);
        }
        
        if(Trigger.isUndelete){
            ContactTriggerHandler.updateContactCount(Trigger.new);
        }
    }
}

// File Name :
public class ContactTriggerHandler {
    
    public static void updateContactCount(List<Contact> conList){
        List<Account> accList = new List<Account>();
        Set<Id> accIdSet = new Set<Id>();
        
        for(Contact con: conList){
            if(con.AccountId != null){
                accIdSet.add(con.AccountId);
            }
        }
        
        for(Account acc: [SELECT Id, (SELECT Id FROM Contacts) FROM Account WHERE Id IN: accIdSet]){
            acc.Contact_Count__c = acc.Contacts.size();
            accList.add(acc);
        }
        
        if(!accList.isEmpty()){
            update accList;
        }
    }
}
```

## DML (Data Manipulation Language)
- DML is used to insert, update, delete & undelete records.
- Use upsert to either insert or update a record.
- Always perform DML in Bulk.
- Handle Exceptions.

**Insert Records**
- Insert only one record.
- Fetch Id just after insertion of record.
- Insert records in bulk using List.
- Governor limits check :
    - Inserting two records separately
    - Insert two records through a List.
- Insert related records. First insert Account then Contact.
  
> Note: Check newly created records in Org.

```java
public class DMLDemo {
    public static void insertDemo(){
        List<Account> accList = new List<Account>();
        for(integer i = 1; i <= 5; i++){
            Account acc = new Account();
        	acc.Name = 'Test DML 2000' + i;
            accList.add(acc);
        }
        insert accList;
        system.debug('Account Id : '+ accList);
    }
}
```

**Update Records**
- Query existing records and display their values.
- Then update the record and do Update DML.
- Check updated records in Org.


## SOQL Cheat Sheet
https://www.apexhours.com/soql-cheat-sheet/

## Asynchronous Processing Basics
- Asynchronous Apex is used to run processes in a separate thread, at a later time. An asynchronous process is a process or function that executes a task "in the background" without the user having to wait for the task to finish.
- Synchronous Apex means entire Apex code is executed in one single go. Asynchronous term means not existing or occurring at the same time.
- We use asynchronous apex mostly when callouts to external systems is required, code needs to be run at some particular time, higher Governor Limits are required for some operation.
- An asynchronous process executes a task in the background.
- User doesn't have to wait for the task to finish.
- Use Asynchronous Apex for:
    - Callouts to external systems
    - Operations that require higher limits
    - Code that needs to run at a certain time.
      
**Benefits of Asynchronous Processing**
- User Efficiency
- Scalability
- Higher Governor Limits
- Can process more number of records

**Types of Asynchronous Processing**
Туре | Overview | Common Scenarios
--- | --- | ---
Future Methods | Run in their own thread, and do not start until resources are available </br> <ul><li>Callouts to external Web services</li><li>Operations which can run in their own thread</li><li>To prevent the mixed DML error</li></ul>| Web service callout.
Batch Apex | Run large jobs that would exceed normal processing limits </br> <ul><li>When processing is required on lots of records</li><li>Jobs that need larger query results</li></ul> | Data cleansing or archiving of records
Queueable Apex | Similar to future methods, but provide additional job chaining and allow more complex data types to be used. </br> <ul><li>Processing on Non-primitive data types</li><li>Chaining of jobs</li><li>To start a long-running operation and get an ID for it</li></ul> | Performing sequential processing operations with external Web Services.
Scheduled Apex |  Schedule Apex to run at a specified time. | Daily or weekly tasks.

**Governor & Execution Limits**
- Asynchronous apex provides higher governor and execution limits.
- Number of SOQL is doubled from 100 to 200.
- Total heap size and maximum CPU time are similarly larger for asynchronous calls.
- As you get higher limits with async, also those governor limits are independent of the limits in the synchronous request that queued the async request initially.

https://developer.salesforce.com/docs/atlas.en-us.salesforce_app_limits_cheatsheet.meta/salesforce_app_limits_cheatsheet/salesforce_app_limits_platform_apexgov.htm

**How Async Processing Works?** </br>
- Enqueue
- Persistence
- Dequeue

### Future Method in apex
- Future Apex runs process in a separate thread, at a later time when system resources become available.
- Use @future annotation to create future methods.
- In Synchronous processing, all method calls are made from the same thread and no additional processing can occur until the process is complete.
- Whereas in future method, methods runs asynchronously in its own thread.
- This unblocks users from performing other operations.
- Provides higher governor & execution limits for processing.
- future method never returns anything so we have to use static void keyword when declaring future method.
- You can perform mixed DML use future method.

**Mixed DML**
There are two types of objects in Salesforce.
1) Set Up Objects: Setup objects are the sObjects that affect the user's access to records in the organization. Eg - User, Profile, Permission Set etc.

2) Non Setup Objects : All (Standard and Custom Objects).
- A mixed DML Operation Error comes when you try to perform DML operations on setup and non-setup objects in a single transaction.
- This restriction exists because some sObjects affect the user's record access in the org. You must insert or update these types of sObjects in a different transaction to prevent operations from happening with incorrect access-level permissions.
- For example, you can't update an account and a user role in a single transaction.

**Disadvantages of Future Method :**
- Chaining is not possible so you cannot call a future from another future or batch apex like it is not guaranteed that method will execute in same order as they called.If you want that go for Queueable.
- In a single request. You can make only 50 future calls.
- Cannot Monitor: Method always be used static & void so you cannot return job ID then how can you monitor.
- You cannot call future method using batch but you can call batch using future method.

**Syntax**
```js
global class FutureClass
{
    @future
    public static void myFutureMethod()
    {   
         // Perform some operations
    }
}
```

**Example**
```js
public class CreateAccountAndUser {
    public static void createData(){
        Account ac = new Account ();
        ac.name = 'Salesforce Shiksha';
        insert ac;
    ｝
    
    public static void createuser (){
        List‹Profile> p = [Select id,name from Profile where name = 'Standard User'];
        List<UserRole> uRole = [Select id, name from UserRole where name = 'CEO'];
        User u = new User();
        u.FirstName = 'Salesforce';
        u.LastName = 'Shiksha';
        u.ProfileId = p[0].id;
        u.UserRoleId = uRole[0].id;
        u.Alias = 'sshik';
        u.Email = 'salesforceshiksha676@gmail.com';
        u.Username = 'salesforceshiksha676@gmail.com';
        u.emailencodingkey= 'UTF-8';
        u.languagelocalekey= 'en_US';
        u.localesidkey='en_US';
        u.timezonesidkey='America/Los_Angeles'
        insert u;
    }
}
```

**Things to Remember**
• It can happen that future methods are running in different order as they are called.
• You cannot call a future method from another.
• There is a limit of 50 future calls per Apex invocation. There is an additional limit on the number of calls in a 24-hour period.

### Batch Apex
1) When you are dealing with big data(millions of records) that can exceed normal processing limits then you use Batch there.
2) Batch is most advanced in Asynchronous Apex.
3) In Batch Apex, you can process records asynchronously in batches.
4) Default batch size is 200.
5) Maximum Batch size is 2000.
6) Implements Database.Batchable interface.
7) If you are using Database. QueryLocator it can retrieves 50 million records from Database while Database. Query retrieves 50 k records.

**Database.Batchable interface gives three methods -**
**1. start (one time run) -** This method collects all the records or objects to be passed to **execute** method for further processing.
**2. execute (running depends on number of jobs) -** 
    - This method executes the functionality or run the process.
    - execute method takes Database.BatchableContext(collects the records or object).
    - A list of object (List‹sobject>).
**3. finish(one time run) -** Used to execute post processing operations. (eg : sending an email).

**Advantages/Features of Batch Apex:**
- In finish method you can invoke another batch.
- When 1 record in a batch fails, that whole batch will be rolled back, but other batches will not be affected.
- Batch apex can make callouts to external systems by using Database.AllowsCallouts interface.
- You can track the state of batch execution by using Database.stateful interface.
- You can schedule batch apex class by implementing Schedulable interface.

**Disadvantages of Batch Apex:**
• You cannot call future method from Batch class.
• Only one batch Apex job's start method can run at a time in an org.

**Syntax**
```js
public class ContactBatch implements Database.Batchable<sobject> {
    public Database.QueryLocator start(Database.BatchableContext BC){
        return Database.getQueryLocator(query);
    }
    
    public void execute(Database.BatchableContext BC, List<sobject> scope){
        for (sobject s : scopel{
            s.put(Field,Value);
        }
        update scope;
    }

    public void finish(Database.Batchablecontext BC){
        system.debug('operation performed successfully.')
    ｝
}
```

**Example**
```js
public class ContactBatch implements Database.Batchable<sobject> {
    public Database.QueryLocator start(Database.BatchableContext BC){
        return Database.getQueryLocator('SELECT Id FROM Contact');
    }
    
    public void execute(Database.BatchableContext BC, List<Contact> contactList){
        for(Contact con: contactList){
            con.Status__c = 'Active';
        }
        update contactList;
    }

    public void finish(Database.Batchablecontext BC){
        system.debug('operation performed successfully.');
    }
}
```
**Execute Window**
```
ContactBatch con = new ContactBatch();
Database.executeBatch(con, 10);
```

### Queueable Apex:
1) It is also a type of asynchronous apex.
2) It is similar to future method but provides additional functionality which future method was unable to provide -
a) We can use non-primitive data type in Queueable apex.
b) Chaining is possible.
c) Can Monitor the job because when you submit job by invoking system.enqueueJob method it returns the ID.You can use that ID to monitor.

**Example**
```js
public class ContactQueueable implements Queueable {
    public void execute(QueueableContext context) {
        List<Contact> conList = [Select id, status__c from Contact];
        for (Contact con : conList){
            con.status__c = 'Inactive';
        }
        update conList;
    }
}
```
**Execute Window**
```
ContactQueueable con = new ContactQueueable();
ID JobId = System.enqueueJob(con);
system.debug('Job Id : ' + JobId);
```

### Scheduled Apex
- You can run Apex classes at a specified time.
- Run Maintenance tasks on Daily or Weekly basis.
- Implements Schedulable interface in Apex class.
- It is also a type of asynchronous apex in which you can schedule your jobs daily & weekly.
- The Apex scheduler lets you delay execution so that you can run apex classes at a specified time.
- It schedules a job in future.
- It's a global class that implements Schedulable Interface.
- You can schedule it using "CRON Expression" or using "Schedule apex button" in Salesforce Ul.
- It has only execute method.
- You can have only 100 active or scheduled jobs concurrently.
- You can call future method using schedule apex ;@future(callout=true)

**Syntax**
```java
global class SomeClass implements Schedulable {
    global void execute(SchedulableContext ctx) {
    // write some code
    }
}
```


