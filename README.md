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
6. [How to avoid Recursion in Trigger](#how-to-avoid-recursion-in-trigger)
7. [Order of execution](#order-of-execution)
8. [Apex Test Class](#apex-test-class)
9. [DML (Data Manipulation Language)](#dml-data-manipulation-language)
10. [SOQL Cheat Sheet](#soql-cheat-sheet)
11. [SOSL](#sosl)
12. [Asynchronous Processing Basics](#asynchronous-processing-basics)
   - [Future Method in apex](#future-method-in-apex)
   - [Batch Apex](#batch-apex)
   - [Queueable Apex](#queueable-apex)
   - [Scheduled Apex](#scheduled-apex)
11. [Integration in Salesforce](#integration-in-salesforce)
    - [Postman Integration](#postman-integration)
    - [Connect One Salesforce to another Salesforce account](#connect-one-salesforce-to-another-salesforce-account)
    - [Create Rest API](#create-rest-api)
12. [Deployment](#deployment)
13. [Exception Handling](#exception-handling)
14. [Alternate of SELECT * in Salesforce](#alternate-of-select--in-salesforce)
15. [Javascript Remoting or Remoting or APEX Remoting or Remote Action](#javascript-remoting-or-remoting-or-apex-remoting-or-remote-action)
16. [Static Resources in Salesforce](#static-resources-in-salesforce)
17. [Generate PDF in Salesforce](#generate-pdf-in-salesforce-download-or-send-email)
18. [Security in Apex class](#security-in-apex-class)
19. [Send Email in Salesforce with Apex Method](#send-email-in-salesforce-with-apex-method)
20. [Server Side Validation](#server-side-validation)
21. [Benefits of Test isRunningTest](#benefits-of-test-isrunningtest)
22. [Bulk API](#bulk-api)
23. [Invocable Method](#invocable-method)

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

#### Best Practices
**- One trigger per object** so you don’t have to think about the execution order as there is no control over which trigger would be executed first.
**- Logic-less Triggers –** use Helper classes to handle logic.
**- Code coverage 100%**
**- Handle recursion –** To avoid the recursion on a trigger, make sure your trigger is getting executed only one time. You may encounter the error : ‘Maximum trigger depth exceeded’, if recursion is not handled well.

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

#### What is Trigger.operationType?
The context variable Trigger.operationType is used to get the current System.TriggerOperation enum value.

The following are the values of the System.TriggerOperation enum:
<br/>
AFTER_DELETE<br/>
AFTER_INSERT<br/>
AFTER_UNDELETE<br/>
AFTER_UPDATE<br/>
BEFORE_DELETE<br/>
BEFORE_INSERT<br/>
BEFORE_UPDATE<br/>
<br/>
Example:
```apex
trigger TestSwitch on Account (before insert, after insert) {
    switch on Trigger.operationType {
        when BEFORE_INSERT {
            System.debug('Before Insert');
        }
        when AFTER_INSERT {
            System.debug('After Insert');
        }
        when else {
            System.debug('Something went wrong');
        }
    }
}
```

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

```apex
Question : Write a trigger to check if account email id already exist when user want to insert any entry into account object.

trigger CheckDuplicateEmail on Account (before insert) {
    // Set to store all email IDs of the new records being inserted
    Set<String> newEmailIds = new Set<String>();
    
    // Loop through the records being inserted to collect email IDs
    for (Account acc : Trigger.new) {
        if (acc.Email__c != null) {
            newEmailIds.add(acc.Email__c);
        }
    }
    
    // Query for existing accounts with the same email IDs
    List<Account> existingAccounts = [SELECT Id, Email__c FROM Account WHERE Email__c IN :newEmailIds];
    
    // Set to store the existing email IDs
    Set<String> existingEmailIds = new Set<String>();
    for (Account acc : existingAccounts) {
        existingEmailIds.add(acc.Email__c);
    }
    
    // Check if any new records have an email ID that already exists
    for (Account acc : Trigger.new) {
        if (acc.Email__c != null && existingEmailIds.contains(acc.Email__c)) {
            acc.addError('An account with this email ID already exists: ' + acc.Email__c);
        }
    }
}
```

```apex
// Question : when contact is inserted or deleted then you want the count of contacts in account object.

trigger contactTrigger on Contact (after insert, after delete, after undelete){
    if(Trigger.isAfter){
        if(Trigger.isInsert){
            ContactTriggerHandler.updateAccountCount(Trigger.new); 
        }

        if(Trigger.isDelete){
            ContactTriggerHandler.updateAccountCount(Trigger.old); 
        }

        if(Trigger.isUndelete){
            ContactTriggerHandler.updateAccountCount(Trigger.new); 
        }
    }
}


public class ContactTriggerHandler{
    public static void updateAccountCount(List<Contact> conList){
        Set<Id> accountIds = new Set<Id>();
        List<Account> updateAccList = new List<Account>();


        for(Contact con : conList){
            if(con.AccountId != null){
                accountIds.add(con.AccountId);
            }
        }

        List<Account> accList = [SELECT Id, (SELECT Id FROM Contacts) FROM Account WHERE Id IN: accountIds];
        for(Account acc: accList){
            acc.contact_count__c = acc.Contacts.size();
            updateAccList.add(acc);
        }

        if(updateAccList.size() > 0) {
           update updateAccList; 
        }
    }
}
```

```apex
// Question : Trigger to count number of sibling in contacts associated with an account
trigger ContactTrigger on Contact (after insert) {
    if(Trigger.isAfter){
        if(Trigger.isInsert){
            ContactTriggerHandler.updateContactSibling(Trigger.new);
        }   
    }
}

public class ContactTriggerHandler{
     public static void updateContactSibling(List<Contact> conList){
        
        // To get list of all accounts (companies) involved in this process
        Set<Id> AccountIds = new Set<Id>();
        for (contact c : conList) {
                if(c.AccountId != null) {
				AccountIds.add(c.AccountId);
        	}
    	}
    
	// To maintain which company has how many employees
	List<Account> accList = [SELECT id, name, (SELECT id FROM Contacts) FROM Account WHERE id IN :AccountIds];
	Map<Id, integer> AccountContactMap = new Map<Id, integer>();
        for(Account acc: accList) {
                integer contactCount = acc.contacts.size();
                AccountContactMap.put(acc.id, contactCount);
        }
        
        // To call all employees one by one and setting the sibling count on that
        list<Contact> GetContacts = [SELECT id, SiblingCount_C, AccountId FROM contact WHERE AccountId in : AccountContactMap.keySet()];
        list<Contact> TpUpdate = new list<Contact>();
    
        for (contact c: GetContacts ){
            c.SiblingCount__c = AccountContactMap.get.(c.AccountId) > 0 ? AccountContactMap.get.(c.AccountId) - 1 : 0;
            TpUpdate.add(c);
        }

        update (TpUpdate);
    }
}

>> In the above scenario we can not handle after update trigger because in trigger handler we are updating contacts itself, so if we'll handle after update trigger then it'll go to in a infinite loop (recusion situation will come)
```

```apex
// Question : Write a trigger to prevert update status of Account object, if user will update satus of account object to Closed then first we need to check if all opportunity is closed or not, if all opportunity has been closed then only user can change status of account to closed else throw an error.


 trigger AccountTrigger on Account(before update){
    if(Trigger.isBefore()){
        if(Trigger.isUpdate()){
            AccountTriggerHandler.updateStatus(Trigger.new, Trigger.oldMap)
        }
    }
 }

 public class AccountTriggerHandler{
    public static void updateStatus(List<Account> accList, Map<id, Account> oldAccountMap){

        Set<Id> accIds = new Set<Id>();
         List<Opportunity> oppList = new List<Opportunity>();
        for(Account acc: accList){
            if(acc.status__c == 'Closed' && oldAccountMap.get(acc.Id).status__c != 'Closed'){
                accIds.add(acc.Id);
            }
        }
        if(accIds.size() > 0){
            oppList = [SELECT Id, Status FROM Opportunity WHERE AccountId IN :accIds AND status__c != 'Closed'];
            Map<Id, Boolean> accOpenOpp = new Map<Id, Boolean>();

            for(Opportunity opp : oppList){
                accOpenOpp.put(opp.AccountId, true);
            }

            for(Account acc: accList){
                if(accOpenOpp.containsKey(acc.Id)){
                    acc.addError('You can not close account because all opportunity should be closed first.');
                }
            }
        }
    }
 }

```

```apex
// Question : Write a trigger to update contact when accounts phone changed.
trigger UpdateContactOnAccountPhoneChange on Account (after update) {
    // Set to hold account IDs with changed phone numbers
    Set<Id> changedAccountIds = new Set<Id>();
    
    // Map to hold account IDs and their new phone numbers
    Map<Id, String> accountIdToPhoneMap = new Map<Id, String>();
    
    // Iterate through the trigger's new list of updated accounts
    for (Account updatedAccount : Trigger.new) {
        // Check if the phone number has been changed
        if (Trigger.oldMap.get(updatedAccount.Id).Phone != updatedAccount.Phone) {
            // Add account ID to the set of changed IDs
            changedAccountIds.add(updatedAccount.Id);
            // Add account ID and new phone number to the map
            accountIdToPhoneMap.put(updatedAccount.Id, updatedAccount.Phone);
        }
    }
    
    // Query for related contacts in bulk
    List<Contact> relatedContacts = [SELECT Id, AccountId FROM Contact WHERE AccountId IN :changedAccountIds];
    
    // Iterate through related contacts and update phone numbers
    for (Contact relatedContact : relatedContacts) {
        relatedContact.Phone = accountIdToPhoneMap.get(relatedContact.AccountId);
    }
    
    // Update contacts
    if (!relatedContacts.isEmpty()) {
        update relatedContacts;
    }
}
```

#### Q: When Maximum Trigger Depth Exceeded Error comes ?
Ans : When trigger goes to recursive loop, that means trigger called itself back to back. we can solve this by static boolean variable (see resolve recursion)

```apex
// Question : we have a picklist of contact type in contact object where we will store type of contact (Platinum, Gold, Silver) and we have one field in account object that is Premium_contact_status. so ask is if any premium contact will update then status value (premium_contact_status__c) should be update.

trigger ContactMasterTrigger on Contact (after update) {
	
    Set<Id> setAccountIds = new Set<Id>();
    List<Account> accountListToBeUpdate = new List<Account>();
    
    for(Contact con: Trigger.new) {
        if(con.AccountId != null && con.contact_type__c == 'Platinum') {
            setAccountIds.add(con.AccountId);
        }
    }
    
    if(setAccountIds.size() > 0){
        List<Account> accList = [SELECT Id, premium_contact_status__c FROM Account WHERE id IN : setAccountIds];
        for(Account acc : accList){
            acc.premium_contact_status__c = 'Platinum contact modified!';
            accountListToBeUpdate.add(acc);
        }
    }
    
    if(accountListToBeUpdate.size() > 0){
        update accountListToBeUpdate;
    }
}
```



## How to avoid recursion in Trigger
Recursion is the process of executing the same Trigger multiple times to update the record again and again due to automation. There may be chances we might hit the Salesforce Governor Limit due to Recursive Trigger.

#### Resolve this issue by using Static Boolean Variable
You can create a class with a static Boolean variable with a default value of true. In the trigger, before executing your code keep a check that the variable is true or not. Once you check make the variable false.

Apex Class with Static Variable

```apex
public class DealTriggerHandler{
     public static Boolean isFirstTime = true;
}
```

Public = so i can use this variable outside
static = so that data will not vanish when control is out of class.

Trigger Code
```apex
Trigger ContactTriggers on Contact (after update){
    Set<String> accIdSet = new Set<String>(); 
    if(DealTriggerHandler.isFirstTime){
        DealTriggerHandler.isFirstTime = false;
        System.debug('---- Trigger run ---->'+Trigger.New.size() );
         for(Contact conObj : Trigger.New){
            if(conObj.name != 'Test') {
                accIdSet.add(conObj.accountId);
            }
         }
       // any code here
    }
}
```

**This is good for less than 200 records. But what about if we have a large set of records it won’t work. It will update the first 200+ records then it will skip the others. Let’s execute the above code with more than 200+ records.**

### Use Static Set or Map
So It is better to use a Static set or map to store all executed record ids. So when next time it will execute again we can check record is already executed or not. Let modify the above code with the help of Set.

```apex
public class ContactTriggerHandler{
     public static Set<Id> setExecutedRecord = new Set<Id>();
}
```

Let us the same set variable in Trigger.

```apex
trigger ContactTriggers on Contact (after update){
        System.debug('---- Trigger run ---->'+Trigger.New.size() );
         for(Contact conObj : Trigger.New){
             if(ContactTriggerHandler.setExecutedRecord.contains(conObj.id)){
                ContactTriggerHandler.setExecutedRecord.add(conObj.id);
                if(conObj.name != 'Test') {
                    // logic
                }
            }    
         }
       // any code here
}
```

## Order of execution
1. Standard validation rule
2. Before trigger
3. Custom validation rule
4. After trigger
5. WFR (Work Flow Rule)
6. Process builder *
7. If WER/PB has any action called as "field update" and this changes any record, and on that record's object we have trigger, then one more time the before and after trigger code will work. BUT ONLY ONE TIME !!!
8. Data will be committed in database

##### IMP IQ:
**Requirement:** If Age < 18 = throw error as per Validation Rule. <br/> <br/>
Assume, user created a record with exp 20, so VR did not come, then trigger came, then WFR came, and at that time by "WFR field update", value of Age became 15, what will happen? Error will come or not?

Answer : Error will not come because after WFR trigger will render.


Counter IQ:
**Requirement :** If Age < 18 = throw error as per "Trigger", using function addError.<br/> <br/>
Assume, user created a record with exp 20, then WFR came, and at that time by "WFR field update", value of Age became 15, what will happen? Error will come or not?

### Order of execution in Visualforce page : 

Visualforce page:
```apex
<apex:page controller="SampleClass" action="{!FunctionA}">
	<apex:outputText>{!VariableABC}</apex:outputText>
</арех:page>
```

```apex
public with sharing class SampleClass {
	public String VariableABC {
		get {
		integer a = btc;
		return a;
		}
		set;
	}

	Or

	//VariableABC {set;get;}

	public SampleClass (ApexPages. StandardController StdController1) {
		// any code
	}

	public void FunctionA() {
		// any code
	}
}
//any code
```
>> Order of execution in visualforce page is 1: constructor function 2: action function 3: getter function


## Apex Test Class
### Introduction
- The Apex testing framework enables you to write and execute tests for your Apex classes and triggers on the Lightning Platform.
- Apex unit tests ensure high quality for your Apex code and let you meet requirements for deploying Apex.
- Apex code can only be written in a sandbox environment (copy of production) or a Developer org, not in production.
- Apex code can be deployed to a production org from a sandbox. Also, app developers can distribute Apex code to customers from their Developer orgs by uploading packages to the Lightning Platform AppExchange (AppExchange is the leading enterprise cloud marketplace).
- In addition to being critical for quality assurance, Apex unit tests are also requirements for deploying and distributing Apex.

### Benefits of Apex Unit Tests
- Ensuring that your Apex classes and triggers work as expected.
- Having a suite of regression tests that can be rerun every time classes and triggers are updated to ensure that future updates you make to your app don't break existing functionality.
- Meeting the code coverage requirements for deploying Apex to production or distributing Apex to customers via packages.
- High-quality apps delivered to the production org, which makes production users more productive.
- High-quality apps delivered to package subscribers, which increase your customers trust.

### Code Coverage Requirement for Deployment
- Before you can deploy your code or package it for the Lightning Platform AppExchange, at least 75% of Apex code must be covered by tests, and all those tests must pass.
- In addition, each trigger must have some coverage. Even though code coverage is a requirement for deployment, don't write tests only to meet this requirement.
- Make sure to test the common use cases in your app, including positive and negative test cases, and bulk and single-record processing.

### Important to Remember
- Calls to System.debug are not counted as part of Apex code coverage.
- Test methods and test classes are not counted as part of Apex code limit (The default Apex code limit in Salesforce is 6 MB of code per organization, excluding comments and @isTest classes). So, no worries about writing long test class with more methods just to make sure that all your code branches are covered.
- Class can be deployed on 0% coverage as well, but that overall coverage of your production org after getting your code deployed should be 75%, otherwise Salesforce won't let you deploy your code.

### Test Method Syntax
```apex
@isTest
static void testName() {
	// code_block
}

OR

static testMethod void testName() {
	// code_block
}
```

### Test Class with Test Method
```apex
@isTest
private class MyTestClass {
	@isTest
	static void myTestMethod() {
		// code_block
	}
}
```

### Examples
FileName : TemperatureConverter.apxc
```apex
public class TemperatureConverter {
	// Takes a Fahrenheit temperature and returns the Celsius equivalent.
	public static Decimal FahrenheitToCelsius(Decimal fh) {
		Decimal cs = (fh - 32) * 5/9;
		return cs.setScale(2);
	}
}
```

FileName : TemperatureConverterTest.apxc
```apex
@isTest
private class TemperatureConverterTest {
	@isTest
	static void testWarmTemp() {
		Decimal celsius = TemperatureConverter.FahrenheitToCelsius(70);
		System.assertEquals(21.11, celsius);
	}

    @isTest
    static void testFreezingPoint() {
        Decimal celsius = TemperatureConverter FahrenheitToCelsius(32) ;
        System. assertEquals (0,celsius);
    }
    
    @isTest
    static void testBoilingPoint() {
        Decimal celsius = TemperatureConverter. FahrenheitToCelsius(212) ;
        System. assertEquals (10,celsius, 'Boiling point temperature is not expected.');
    ｝

    @isTest
    static void testNegativeTemp() {
        Decimal celsius = TemperatureConverter. FahrenheitToCelsius(-10) ;
        System. assertEquals (-23.33,celsius);
    ｝
}
```

Example for Trigger : after insert in account Object create opportunity (above trigger example : AccountTriggerHandler.apxc -> CreateOpportunity())
```apex
@isTest
private class AccountTriggerHandlerTest {
    @isTest
    static void createOppTest (){
        List<Account> accList = new List<Account>();

        for(Integer i=1; i<=10; i++){
            Account acc = new Account ();
            acc.Name = 'Test ' + i;
            acc.Active__c = 'Yes';
            accList.add (acc) ;
        }

        Test.startTest();
        insert accList;
        Test. stopTest() ;

        List<Opportunity> oppList = [Select Id FROM Opportunity WHERE AccountId =: accList[0].Id];
        System.assert(oppList != null, 'Opportunity is not created');
    }

    @isTest
    static void populateRatingTest() {
        List<Account> accList = new List<Account>();

        for(Integer i=1; i<=10; i++){
            Account acc = new Account ();
            acc.Name = 'Test ' + i;
            acc.Active__c = 'Yes';
            acc.Industry = 'Media';
            accList.add (acc) ;
        }

        Test.startTest();
        insert accList;
        Test. stopTest() ;

        List<Account> updatedAccountInfo = [Select Rating FROM Account WHERE Id =: accList[0].Id];
        System.assertEquals('Hot', updatedAccountInfo[0].Rating);

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

## Database.insert (process Bulk DML)

1. Helpful in processing in large records
2. When we want to see which record were successful
3. When we want to see which records failed
4. What are IDs of success records
5. What are error reasons
6. What are error fields
7. This will help for partial DML
8. We can process large records by this
9. Documentation can be done using debig logs to know the reasons of errors
10. Everything is dynamic, whether records are few or large
<br/><br/>

DML statement with database function:
Suppose, we have some records (say, 20), in a list. If suppose, I did insert for that list, and 1 record is having bad data so error took place. Yet, all rest 19 records will be cancelled. And nothing will be inserted.
<br/>
This is the regular pattern of DML.

When we make any transaction using **database.insert()** or **database.update()** then the return value is **database.saveresult**

```apex
Account acc1 = new Account();
acc1.Name = 'Account 22001';


Account acc2 = new Account();
acc2.Name = 'Account 22001';


Account acc3 = new Account();
acc3.Industry = 'Chemical';

list<Account> accList = new list<Account>();
accList.add(acc1);
accList.add(acc2);
accList.add(acc3);

List<database.saveresult> resultList = database.insert(accList, false);
system.debug(resultList);
for(database.saveresult result : resultList){
    system.debug('--------------------------');
    system.debug(result.isSuccess());
    system.debug(result.getErrors());
    system.debug(result.getID());
    system.debug('--------------------------');
}

```

<img width="1435" alt="Screenshot 2024-06-09 at 10 42 25 AM" src="https://github.com/therishabh/salesforce-apex/assets/7955435/73149346-79c4-4ba3-adad-f6137247a53d">

database.saveresult database contains 3 methods 
- getErrors()
- getId()
- isSuccess()
  
![Screenshot 2024-06-09 at 10 46 09 AM](https://github.com/therishabh/salesforce-apex/assets/7955435/e83dd55f-7eda-47f2-87c2-4b53141c9997)



## SOQL Cheat Sheet
https://www.apexhours.com/soql-cheat-sheet/

## SOSL
Salesforce Object Search Language (SOSL) is a Salesforce search language that is used to perform text searches in records. Use SOSL to search fields across multiple standard and custom object records in Salesforce. SOSL is a search language in salesforce and the important feature is that Unlike SOQL, we can search in multiple objects at same time using SOSL. In SOQL, we can query only one object at a time but in SOSL, We can search for some specified string like ‘testString’ in multiple objects at the same time.

#### Difference between SOQL and SOSL
Like SOQL, SOSL allows you to search your organization’s records for specific information. Unlike SOQL, which can only query one standard or custom object at a time, a single SOSL query can search all objects.

Another difference is that SOSL matches fields based on a word match while SOQL performs an exact match by default (when not using wildcards). For example, searching for ‘Digital’ in SOSL returns records whose field values are ‘Digital’ or ‘The Digital Company’, but SOQL returns only records with field values of ‘Digital’.

#### Example
```apex
// Search 'Salesforce' keyword in Account object under all fields
List<List<SObject>> accList = [FIND 'Salesforce' IN ALL FIELDS RETURNING Account];

// Search 'Salesforce' keyword in Account object under all fields and get it's ID, Name, Phone field info
List<List<SObject>> accList = [FIND 'Salesforce' IN ALL FIELDS RETURNING Account(Id, Name, Phone)];

// Search 'Salesforce' keyword in Account object under all fields and get it's ID, Name, Phone field info and we need to add where condition for account object
List<List<SObject>> accList = [FIND 'Salesforce' IN ALL FIELDS RETURNING Account(Id, Name, Phone WHERE Name = 'Manchester Utd')];

// if we want to search from two objects
List<List<SObject>> accList = [FIND 'Salesforce' IN ALL FIELDS RETURNING Account(Id, Name, Phone), Contact(Id, Name)];

// if you want you search multiple text in single query then we need to pass both keyword in single quote by OR seperator
List<List<SObject>> accList = [FIND '"Salesforce" OR "SF"' IN ALL FIELDS RETURNING Account(Id, Name, Phone), Contact(Id, Name)];

// wildcards:
FIND {Salesforce N*}
FIND {Salesforce N?ob}

List<List<SObject>> accList = [FIND 'Sa*' IN ALL FIELDS RETURNING Account(Id, Name, Phone), Contact(Id, Name)];

```

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

    @future
    public static void createuser (){
        List‹Profile> upro = [Select id,name from Profile where name = 'Standard User'];
        List<UserRole> uRole = [Select id, name from UserRole where name = 'CEO'];
        User u = new User();
        u.FirstName = 'Salesforce';
        u.LastName = 'Shiksha';
        u.ProfileId = upro[0].id;
        u.UserRoleId = uRole[0].id;
        u.Alias = 'sshik';
        u.Email = 'salesforceshiksha676@gmail.com';
        u.Username = 'salesforceshiksha676@gmail.com';
        u.emailencodingkey= 'UTF-8';
        u.languagelocalekey= 'en_US';
        u.localesidkey='en_US';
        u.timezonesidkey='America/Los_Angeles';
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

**Database.Batchable interface gives three methods -**</br>
**1. start (one time run) -** This method collects all the records or objects to be passed to **execute** method for further processing.</br>
**2. execute (running depends on number of jobs) -** </br>
    - This method executes the functionality or run the process.</br>
    - execute method takes Database.BatchableContext(collects the records or object).</br>
    - A list of object (List‹sobject>).</br>
**3. finish(one time run) -** Used to execute post processing operations. (eg : sending an email).</br>

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

Q. : How to check failed records in batchable apex?</br>
A.</br>
first of all you need to make your batch class stateful using Database.Stateful so replace your first line with</br></br>

```public class SimpleBatch implements Database.Batchable<sObject>,Database.Stateful{```

A global variable required which will maintain failed record.

```global List<String> exception_List;```

Use Database.update method instead of update with allOrNothing = false parameter which will return how many records has passed and failed in update call.

```Database.SaveResult[] SaveResultList = Database.update(objsToUpdate,false);```

You will iterate saveResultList and you will add failed records in exception_list

```apex
for(integer i = 0; i < objsToUpdate.size(); i++) {
        String msg=''; 
        If(!SaveResultList[i].isSuccess()) {
                msg += userList.get(i).id + '\n'+'Error: "';        
                for(Database.Error err: SaveResultList[i].getErrors()){  
                        msg += err.getmessage()+'"\n\n';
                } 
        }
        if(msg!='')
                exception_List.add(msg);
} 
```

You can use this exception_list in execute method to send in your final email.

#### Q: Can we call a batch class from another ?
Ans : Yes</br></br>
How ?</br>
In finish method</br>

#### Q. Can we call a batch class from trigger ?
Ans.  Yes, but not advised because there is a limit on calling batch classes per Salesforce org. If Trigger has large records, it will call many batch classes, then limit can hit.</br>
Upto 5 active batch jobs are allowed in APEX (in queue).

#### Q: What access specifiers we use in Batch class?
Ans. Global

#### What interface we use for Batch apex/Batch class?
Ans. Database.Batchable

#### Q: What is QueryLocator?
Ans. The way we have data type like integer, string; same way we have "Database.QueryLocator" which contains large number of records, like a container/pot.</br></br>
And this is return type of start() function.

#### Q. How we are going to call the query? Normal way?
Ans. No! </br>
We will use "database.getQueryLocator()" function.

#### Q.
#### How to count any values of processed records in batch class ?
#### OR
#### Can I save info about processed records in batch class?
#### OR
#### I want to make sum of all values processed by above batch class?
This IS first line Of Batch class | normal process: <br/>
<br/>
`global class AccountlaxUpdate implements Database.Batchable<sObject> {`
<br/><br/>
Make it like this:<br/>
<br/>
`global class AccountTaxUpdate implements Database.Batchable<sObject>, Database.Stateful{`

#### IQ:TCS, India :  what can be another return type of Start function ?
Ans. Iterator (we dont use this in max cases) (just memorize for interview)

#### Q: How many parallel batch apex can be called in one org?
Ans. 5 (that's why we should not call any batch apex from Trigger)

### Queueable Apex:
1) It is also a type of asynchronous apex.
2) It is similar to future method but provides additional functionality which future method was unable to provide - </br>
	a) We can use non-primitive data type in Queueable apex.</br>
	b) Chaining is possible.</br>
	c) Can Monitor the job because when you submit job by invoking system.enqueueJob method it returns the ID.You can use that ID to monitor.</br>

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
- You can schedule it using **"CRON Expression"** or using "Schedule apex button" in Salesforce Ul.
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

<img width="1222" alt="Screenshot 2024-06-09 at 5 44 44 PM" src="https://github.com/therishabh/salesforce-apex/assets/7955435/52f1e9b1-c78a-4ec1-93a9-f235b936f2ba">
<img width="1214" alt="Screenshot 2024-06-09 at 5 45 48 PM" src="https://github.com/therishabh/salesforce-apex/assets/7955435/931de9ed-a333-41e9-ad8c-efd6d42a490e">

Check scheduled jobs
<img width="1216" alt="Screenshot 2024-06-09 at 5 47 20 PM" src="https://github.com/therishabh/salesforce-apex/assets/7955435/8fcade83-699a-4901-ad6b-77dbe22cf568">

#### Q:Amazon, India : How by query/code we can check above thing?
>> Select Id, CronJobDetail.Name, State, NextFireTime From CronTrigger limit 10

#### IQ:Object which stores all such info: 
Ans: "CronTrigger"

## Integration in Salesforce
**Integration Methods**

Salesforce can be integrated with the other systems by using various methods depending upon the overall business requirement, the commonly used ones are:
- Via Apex Method
- Mulesoft, Informatica and other third party ETL Tools
- Various AppExchange Products

Points to remember before setting up the integration between Salesforce and the other system using any of the above-mentioned methods:
- Document what and how often information needs to move between systems.
- Review API limits
- Identify the type of integration involved: Real-time or batch

#### Webservices
In Salesforce, web services are a set of APIs that allow users to interact with the Salesforce platform. These APIs define a set of features that are accessible over the web. Web services can be used for a variety of functionalities, including:
- Data transfers
- Business process management
- Creating, retrieving, updating, or deleting records
- Real-time integration with third-party devices and apps

Apex web services can be used to expose Apex class methods as REST or SOAP web service operations. This allows external applications to integrate with Salesforce to perform operations such as creating, updating, and deleting records.</br>
The SOAP API and REST API are both considered webservices and APIs in Salesforce. SOAP is a structured protocol, while REST is more flexible and less defined. Both of the API formats use data that both humans and machines can read, and both often use HTTP protocols.

<img width="901" alt="Screenshot 2024-05-21 at 1 38 58 PM" src="https://github.com/therishabh/salesforce-apex/assets/7955435/0c17131e-6f78-441d-87b9-9faf7d4d76e6">

#### What is API ?
API stands for Application Programming Interface and it's simply another way of sending commands to Salesforce, only this time there's no graphical UI to work through, instead developers use the APIs to send commands like View or Save to Salesforce programmatically by writing platform code.

#### What is connected app ?
A connected app is a **framework that enables an external application to integrate with Salesforce using APIs and standard protocols, such as SAML, OAuth, and OpenID Connect**. Connected apps use these protocols to authenticate, authorize, and provide single sign-on (SSO) for external apps.</br>
Connected Apps help user delegate their access without sharing sensitive credentials or giving full control of their applications to third parties. </br>
> Note : The maximum number of OAuth 2.0 connected apps that can be registered through the dynamic client registration endpoint is 100

#### Postman Integration
- To begin, navigate to the setup page in Salesforce and locate the "App Manager" within the setup menu. From there, click on the **New Connected App** button to proceed.
<img width="721" alt="Screenshot 2024-05-21 at 1 58 57 PM" src="https://github.com/therishabh/salesforce-apex/assets/7955435/997b597e-8e24-4593-addf-c167568f07b8">

- Enter all required information, then check the "Enable OAuth Settings" checkbox. Next, enter the callback URL. Now, proceed to select OAuth Scopes from the list. For now, you can select "Full access" and save your settings. Activation may take up to 10 minutes.
![screencapture-dyninno-dev-ed-develop-lightning-force-lightning-setup-ConnectedApplication-page-2024-05-21-14_02_43](https://github.com/therishabh/salesforce-apex/assets/7955435/0e5e7fad-3676-4237-abe7-5bc84106bc4e)

- After creating your connected app, you'll be directed to your app page. Here, click on the **Manage Consumer Details** button. This action will first validate your account by sending a verification code to your email. After verification, you will receive your **Consumer Key** and **Consumer Secret**.
<img width="1440" alt="Screenshot 2024-05-21 at 2 06 44 PM" src="https://github.com/therishabh/salesforce-apex/assets/7955435/2f6925df-8f5b-4610-98fb-f74d343cd888">
<img width="721" alt="Screenshot 2024-05-21 at 2 07 44 PM" src="https://github.com/therishabh/salesforce-apex/assets/7955435/6042d211-966a-4645-97e1-b4f055e1b3cd">

- To generate a Security Token, begin by clicking on your name in the header (classic mode) and navigate to the Settings section in the left-hand sidebar. From there, click on the "Reset My Security Token" link to create your token. Once generated, the token will be sent to your email address.
<img width="721" alt="Screenshot 2024-05-21 at 2 14 20 PM" src="https://github.com/therishabh/salesforce-apex/assets/7955435/151810ce-65d1-4cf4-b4c7-64b2b01a1524">

- Now Jump to our Postman and in postman first we need to create a request for access salesforce and get access_token.
    - Enter URL in postman as base url and then add /services/oauth2/token
    - create a post request.
    - add following fields in body as form-data
        - username : username of salesforce account
        - password : password of salesforce account + Security Token which was sent to your email address.
        - grant_type : 'password'
        - client_id : Consumer Key
        - client_secret : Consumer Secret

<img width="721" alt="Screenshot 2024-05-21 at 2 20 59 PM" src="https://github.com/therishabh/salesforce-apex/assets/7955435/96458216-aada-4841-8fc4-e9cf5c34a41e">


> Note : There is one more setting is important to configure the integration between two Salesforce orgs that should be enabled 'Allow OAuth Username-Password Flows' when getting invalid grant error.
>
> ![VElzx](https://github.com/therishabh/salesforce-apex/assets/7955435/8efd795b-3109-48c6-9da9-c9a32942f2ce)

- After hit that above api you'll get access_token in response and from now you'll send many api by using that access_token in header.

<img width="1439" alt="Screenshot 2024-05-21 at 2 27 24 PM" src="https://github.com/therishabh/salesforce-apex/assets/7955435/ef1130b7-2a98-4c26-839e-ee6ca0a7a228">

### Connect One Salesforce to another Salesforce account

```apex
public class SalesforceInregrationController {
    
    public string instance_url;
    
    public static void getAccessToken() {
    	String endpoint = 'https://dyninno-dev-ed.develop.my.salesforce.com/services/oauth2/token';
        String methodType = 'POST';
        String username = 'rishabhagrawal019@gmail.com';
        String password = 'Q!w2e3uaY9GY5VdmeBdnbkXeRHRnMc';
        String grantType = 'password';
        String clientId = '3MVG9pRzvMkjMb6k97lCSQehB7oWFZw5i.lrmuHTJASFIDNnnadire83dJD_Jjkasdfie393.Qh3jVxE756Ol';
        String clientSecretKey = '6EBB20442F8A7C503AEA73FE259319192AB1FD40DA8F4153A5JASDLKFEIR2348';
        
        // create a new http request
        HttpRequest httpReq = new HttpRequest();
        
        //set all fields into httpReq..
        httpReq.setEndpoint(endpoint);
        httpReq.setMethod(methodType);
        httpReq.setBody(
        	'username=' + username +
            '&password=' + password +
            '&grant_type=' + grantType + 
            '&client_id=' + clientId + 
            '&client_secret=' + clientSecretKey
        );
        
        Http http = new Http();
        HttpResponse httpResp = http.send(httpReq);
       	String jsonBody = httpResp.getBody();
        
        // deserilization
        SalesforceWrapper wrapData = (SalesforceWrapper)System.JSON.deserialize(jsonBody, SalesforceWrapper.class);
        
        system.debug('====> Accesss Token ====> '+ wrapData.access_token);
        system.debug('====> instance_url ====> '+ wrapData.instance_url);
        system.debug('===> response ===> ' + httpResp.getBody());
        getAccountData(wrapData.access_token);
        // createAccount(wrapData.access_token);
        
    }
    
    public Class SalesforceWrapper{
        public String access_token;
        public String instance_url;
        public SalesforceWrapper(String access_token, String instance_url){
            this.access_token = access_token;
            this.instance_url = instance_url;
        }
    }
    
    public static void getAccountData(String accessToken) {
        String url = 'https://dyninno-dev-ed.develop.my.salesforce.com/services/data/v60.0/query/?q=SELECT Id, Name FROM Account ORDER BY CreatedDate DESC LIMIT 5';
    	String methodType = 'GET';
        
        // create a new http request
        HttpRequest httpReq = new HttpRequest();
        httpReq.setEndpoint(url);
        httpReq.setMethod(methodType);
        httpReq.setHeader('Authorization', 'Bearer ' + accessToken);
        httpReq.setHeader('Content-Type', 'application/json');
        
        Http http = new Http();
        HttpResponse response = http.send(httpReq);
        system.debug('get Account Data ====> '+response.getBody());
        
    }
    
    public static void createAccount(String accessToken) {
        String endpoint = 'https://dyninno-dev-ed.develop.my.salesforce.com/services/data/v60.0/sobjects/Account';
    	String methodType = 'POST';
        
        String body = '{"Name" : "Account Shikha Salesforce"}';
        
        // create a new http request
        HttpRequest httpReq = new HttpRequest();
        httpReq.setEndpoint(endpoint);
        httpReq.setMethod(methodType);
        httpReq.setHeader('Authorization', 'Bearer ' + accessToken);
        httpReq.setHeader('Content-Type', 'application/json');
        
        httpReq.setBody(body);
        
        Http http = new Http();
        HttpResponse response = http.send(httpReq);
        system.debug('new Account Data ====> '+response.getBody());
        
    }
}
```

### Create Rest API
**Apex Web Services:**
- We can expose our Apex class methods as a REST or SOAP web service operations.
- By making your methods callable through the web, external applications can integrate with Salesforce to perform all type of operations like Add, Update, Delete etc to and from salesforce.
- What that means is that you can create custom web services to make your APEX methods available to complete APEX code.

**Syntax**
To make your Apex class available as a REST web service is straightforward.
- Define your class as global,
- and define methods as global static.
- Add annotations to the class and methods.

For example, this sample Apex REST class uses one method. The getRecord method is a custom REST API call. It's annotated with **@HttpGet** and is invoked for a GET Request.
```apex
@RestResource(urlMapping='/Account/*')
global with sharing class MyRestResource{
    @HttpGet
    global static Account getRecord(){
        // Add your code...
    }
}
```

The class is annotated with **@RestResource(urlMapping='/Account/*')**

### RestContext Class and Properties</br>

- Contains the RestRequest and RestResponse objects.
**RestContext Properties -** The following are properties for RestContext.

**- request:** Returns the RestRequest for your Apex REST method.
**- response :** Returns the RestResponse for your Apex REST method.


**Base URL of Apex Rest**
- The base endpoint for Apex REST is `https://yourInstance.my.salesforce.com/services/apexrest/`
- The URL mapping is appended to the base endpoint to form the endpoint for your REST service.
     For example, in the class example, the REST endpoint is `https://yourinstance.my.salesforce.com/services/apexrest/`
- For your org, it could look something like,
`https://yourInstance.my.salesforce.com/services/apexrest/Account/*`
- The URL mapping is case-sensitive and can contain a wildcard character (*)
-  Note: You can use each annotation only once in each Apex class.
-  Different Types of Annotations available

**Different Type of Annotations Available**
<img width="812" alt="Screenshot 2024-05-21 at 3 49 14 PM" src="https://github.com/therishabh/salesforce-apex/assets/7955435/9d6c9a42-666e-4c1f-b4c6-2b4a53836ccd">

Apex provides mainly three built in classes to work with HTT services and create HTTP requests:</br>
**- Http Class:** It is used when initiating an HTTP request and response.</br>
**- HttpRequest Class:** It is used when initiating HTTP requests such as GET, DELETE, POST, PUT, and PATCH.</br>
**- HttpResponse Class:** It is used when handling the HTTP response returned by HTTP</br></br>

Example

For single record
```apex
@RestResource(urlMapping='/createAccountRecord')
global class MyRestWebService {
	@HttpPost
    global static void sendAccountRecords(){
        RestRequest request = RestContext.request;
        RestResponse response = RestContext.response;
        
        AccountWrapper requestBody = (AccountWrapper)JSON.deserialize(request.requestBody.toString(), AccountWrapper.class );
        if(requestBody != null){
            Account acc = new Account();
            acc.Name = requestBody.name;
            acc.Rating = requestBody.rating;
            acc.Type = requestBody.type;
            if(acc != null){
                insert acc;
            }
        }
        
    }
    
    global Class AccountWrapper{
        global String name;
        global String rating;
        global String type;
    }
}
```

For Multiple Records
```apex
@RestResource(urlMapping='/createAccountRecord')
global class MyRestWebService {
	@HttpPost
    global static void sendAccountRecords(){
        RestRequest request = RestContext.request;
        RestResponse response = RestContext.response;
        
        String responseJson = '[';
        List<Account> accountList = new List<Account>();        
        List<AccountWrapper> requestBody = (List<AccountWrapper>)JSON.deserialize(request.requestBody.toString(), List<AccountWrapper>.class );
        
        if(!requestBody.isEmpty() && requestBody.size() > 0){
            for(AccountWrapper accObj : requestBody){
                Account acc = new Account();
                acc.Name = accObj.name;
                acc.Rating = accObj.rating;
                acc.Type = accObj.type;
                accountList.add(acc);
            }
            
            if(!accountList.isEmpty() && accountList.size() > 0){
            	insert accountList;
                for(Account ac : accountList){
                    responseJson += '{ "record id" : "'+ac.Id+'", "status" : "success"},';
                }
                responseJson = responseJson.removeEnd(',');
            }
        }
        responseJson += ']';
        response.responseBody = Blob.valueOf(responseJson);
        response.statusCode = 200;
    }
    
    global Class AccountWrapper{
        global String name;
        global String rating;
        global String type;
    }
}
```

## Deployment

QA org => SIT org => UAT org => Production

### Introduction
- In Salesforce, "deployment" typically refers to the process of moving changes made in one environment (such as development or sandbox) to another sandbox or production environment.
- The deployment process ensures that any new configurations, customizations, or code changes are safely and correctly applied to the production instance of Salesforce without causing any disruption to the existing functionality.

### Data Vs Metadata
- Metadata is the data which describes other data.
	- Example : Field APIs, Object APIs
- Data is actual information.
	- Example: Values in particular field of a record

### Sandbox:
When we make payment to SF to get the environment to work, we get Production. We must not do our work directly on Production as it hampers the work of client. 

So we need to create Sandbox first.

#### What is Sandbox exactly?
It's nothing but the replica of Production environment with some limitations depending on type of Sandbox, and in Sandbox we actually need to make implementation of any Project work we do.

#### A deployment related real time scenario:

1 Jan = Got project </br>
2 Jan = Client bought SF edition / PROD </br>
3 Jan = Sandbox created | We started work </br>
1 Feb = Project done | Deployed to PROD </br>
2 Feb = Hereonwards client started using project & asked us to stop the service </br>
.</br>
.</br>
.</br>
.</br>
.</br>
.</br>
.</br>
.</br>
1 Aug = Client told some changes in present code </br>
2 Aug = Started working on PROD ? = Wrong = Why? </br>
2 Aug = Started working on Sandbox = Wrong = Why? </br>
</br>
</br>
PROD to SB = Refresh </br>
When we need fresh copy of LIVE env. </br>
</br>
</br>
SB to PROD = Deploy </br>
When our work is done, and we need to provide to client. </br>

### Salesforce Sandboxes
There are 4 Sandboxes
- Developer
- Developer Pro
- Partial
- Full Copy (More or less a replica of Production Environment)

#### Developer Sandbox Characteristics
- Refresh Interval - 1 Day
- Data Storage - File Storage - 200MB | Data Storage - 200MB
- What's Copied? - Metadata

#### Developer Pro Sandbox Characteristics
- Refresh Interval - 1 Day
- Data Storage - File Storage - 1GB | Data Storage - 1GB
- What's Copied? - Metadata

#### Partial Sandbox Characteristics
- Refresh Interval - 5 Days
- Data Storage - File Storage - 5GB | Data Storage - 5GB
- What's Copied? - Metadata + Sample Data

#### Full Copy Sandbox Characteristics
- Refresh Interval - 29 Days
- Data Storage - File Storage - 5GB(+more) | Data Storage - 5GB(+more)
- What's Copied? - Metadata + All Data

### Sandbox Development Flow
<img width="1094" alt="Screenshot 2024-05-26 at 9 59 42 PM" src="https://github.com/therishabh/salesforce-apex/assets/7955435/83677e5d-07ca-4168-9057-2d339f19cc44">


### Change Set:
Change set is like a suitcase.</br>
Change set helps us to transfer the items from SB to PROD where all the entities will be added to CS by person at SB. And this CS will be received by person at Prod.
</br></br>
Types:</br>
1. Inbound CS</br>
2. Outbound CS</br>

#### Login on Sandbox: 

Production:</br>
URL = http://login.salesforce.com</br>
UID = therishabhagrawal@gmail.com</br>
pw = abc</br>
</br>
==</br>
</br>
Sandbox:</br>
(Assume, sandbox name is 'sandbox1')</br>
URL = http://test.salesforce.com</br>
UID = therishabhagrawal@gmail.com.sandbox1</br>
pw = abc</br>
</br>

#### Create new Sandbox from Production account
- Click on setup then you'll see sandbox in left pannel
  <img width="721" alt="Screenshot 2024-06-04 at 3 09 40 AM" src="https://github.com/therishabh/salesforce-apex/assets/7955435/13d0ce4c-2aa4-4bb1-b57c-07bf295af212">

- Sandbox screen
  <img width="1440" alt="Screenshot 2024-06-04 at 3 09 31 AM" src="https://github.com/therishabh/salesforce-apex/assets/7955435/e4a93219-1872-4572-8d2c-129e3eb38eda">

- <img width="1439" alt="Screenshot 2024-06-04 at 3 12 18 AM" src="https://github.com/therishabh/salesforce-apex/assets/7955435/722b4342-e1d2-4aa3-9754-de7c0bb0c349">

- <img width="722" alt="Screenshot 2024-06-04 at 3 12 46 AM" src="https://github.com/therishabh/salesforce-apex/assets/7955435/f95aadfc-7415-4109-9dd4-df66abe80fef">

<img width="1438" alt="Screenshot 2024-06-04 at 3 13 02 AM" src="https://github.com/therishabh/salesforce-apex/assets/7955435/97076218-fbde-4410-824e-f356a28ea29f">

<img width="1437" alt="Screenshot 2024-06-04 at 3 13 25 AM" src="https://github.com/therishabh/salesforce-apex/assets/7955435/fe756086-addc-408c-aad5-0ef28b392f7c">

<img width="1440" alt="Screenshot 2024-06-04 at 3 15 12 AM" src="https://github.com/therishabh/salesforce-apex/assets/7955435/5b3a2880-6826-41b6-b130-975ee7253f84">

<img width="1436" alt="Screenshot 2024-06-04 at 3 18 59 AM" src="https://github.com/therishabh/salesforce-apex/assets/7955435/ddbd3799-22ea-49e2-aa80-a04f0e907290">

<img width="1438" alt="Screenshot 2024-06-04 at 3 19 54 AM" src="https://github.com/therishabh/salesforce-apex/assets/7955435/210e800c-ab1c-4992-96b1-df645c314424">

Before creating outbound set First of all we need to create a connection between our sandbox with production, so for that we need to click on deployment settings.
<img width="1440" alt="Screenshot 2024-06-04 at 3 21 12 AM" src="https://github.com/therishabh/salesforce-apex/assets/7955435/a5df9edf-3a30-49ab-ac9b-d2a082e440a8">
<img width="722" alt="Screenshot 2024-06-04 at 3 24 20 AM" src="https://github.com/therishabh/salesforce-apex/assets/7955435/fa761807-6bd6-4896-b7c2-07753ea2b378">
<img width="722" alt="Screenshot 2024-06-04 at 3 24 32 AM" src="https://github.com/therishabh/salesforce-apex/assets/7955435/7cb1554b-798e-4f46-92af-2b7962703fa6">

This is production screen (Deployment Screen)
<img width="1431" alt="Screenshot 2024-06-04 at 3 25 22 AM" src="https://github.com/therishabh/salesforce-apex/assets/7955435/c60aba11-2755-4d3d-8b88-0930263c64ae">
<img width="721" alt="Screenshot 2024-06-04 at 3 26 44 AM" src="https://github.com/therishabh/salesforce-apex/assets/7955435/c7178ede-2f73-46f4-82a6-d724ef6c6b02">
<img width="721" alt="Screenshot 2024-06-04 at 3 27 01 AM" src="https://github.com/therishabh/salesforce-apex/assets/7955435/e1a874b1-0390-407e-9e7f-eedb6296ac84">

#### Video Link : 
https://www.youtube.com/watch?v=EAudzGsZoq8&ab_channel=ShekharSalesforce



### Various Deployment Methods
- Change Set
- Workbench
- Salesforce CLI (Command Line Interface) - ANT
- Salesforce Extension for VS Code
- Salesforce DX
- Metadata API
- Managed Packages
- Continuous Integration/Continuous Deployment (CI/CD)
- 3rd Party Tools - Copado, Flosum, Gearset

#### Change Set
- Change Sets are a simple and point-and-click way to transfer customizations between Salesforce organizations.
- They are typically used for moving changes from sandbox to production or between sandboxes.
- Change Sets can include various components such as custom objects, fields, workflows, reports, and more.
- Change set is provided by Salesforce itself, so we don't need to install anything it will be available inside our salesforce.

#### Workbench
- Workbench is a powerful web-based tool and a popular choice among Salesforce developers and administrators for various tasks, including deployment.
- It provides a user-friendly interface to interact with Salesforce metadata and data, making it easier to perform operations such as data manipulation, metadata deployment, querying, and more.

#### Salesforce CLI
- Salesforce CLI allows developers to use command-line commands to retrieve and deploy metadata between different environments.
- This method is more flexible and can be used in combination with version control systems, such as Git, for better development practices.

#### Salesforce Extensions for VS Code
- This is an extension in Visual Studio Code that provides powerful development and deployment tools for Salesforce developers.
- It enables source code management, deployment, and other development tasks within the integrated development environment (IDE).

#### Salesforce DX (Developer Experience)
- Salesforce DX is a set of tools and practices aimed at improving the development lifecycle.
- It allows for source-driven development, meaning that the source code is the single source of truth for the application.
- It uses scratch orgs for development and facilitates easy deployment to production or other environments.

#### Metadata API
- The Metadata API is a web service provided by Salesforce, which allows you to retrieve and deploy metadata components programmatically.
- This API is suitable for automating deployments and integrating with external tools or systems.

#### Managed Package
- If you want to distribute your custom applications to other Salesforce organizations, you can package your components into Managed Packages.
- Managed Packages can be installed in other orgs, and they help you ensure that your customizations are secure and upgradeable.

#### CI/CD
- For a more automated and continuous deployment process, you can set up Cl/CD pipelines using tools like Jenkins, CircleCI, or Azure DevOps.
- These pipelines automate the deployment process, running tests and deploying changes to production whenever new code is pushed to a specific branch.

#### 3rd Party Tools - Copado
- Copado is a popular native Salesforce Continuous Integration and Continuous Deployment (Cl/CD) platform that aims to streamline and automate the development and deployment processes for Salesforce applications.
- It is designed to work natively within the Salesforce ecosystem and provides a set of tools and features that enable efficient application development, testing, and deployment on the Salesforce platform.

Youtube Link regarding change set, Outbonund and Inbound. : 
Deployment Using change set, Outbound and Inbound changeset
******https://www.youtube.com/watch?v=GIVbdVwjdag******


## Change Sets in Salesforce
A change set is a means by which one organization can send customizations to another organization. Change sets in Salesforce are groupings of components that can be deployed from one Salesforce org to another Salesforce org. This is out of a box deployment user interface tool provided by Salesforce. Change sets contain information about the org. They don’t contain data, such as records.

#### What is an outbound change set?
**Outbound change set:** An outbound change set is a change set created in the Source organization that you want to deploy to the target organization. Sending an outbound change set to the target org doesn’t mean that the changes will be deployed in that org. The change set must be accepted and deployed.

#### What is an inbound change set?
**Inbound change set:** An inbound change set is a change set that is sent from the source Salesforce org to the target Salesforce org.


## Exception Handling
Whenever any code fails or breaks, then SF throws an error on the screen of user.<br/>

This message is mostly raw and gives no clarity to user. Also user gets unclear information, which confuses him more.<br/>

The ideal scenarios is, whenever any error occurs, then we need to handle it and present a user friendly message for the same Or inform concern technical person like Developer about this issue.<br/>

To handle this situation, first you need to know, what kind of errors are possible? <br/>
Means types of Exceptions :<br/>

Type of exception: <br/>
1. DML exception
2. List exception
3. Null pointer exception
4. Query exception
5. Generic type


How to write code here after to handle the exception:

```apex
try {
	//code from which u think err will come
｝
catch {
	// instead of throwing err on screen, write catch block here, so SF will not throw err on screen and SF will assume that u will handle this situation and SF will realize it has no responsibility wont do anything.
	// What we can go ?
	// Show some simple message on screen / send email to developer etc anything
}
finally{
	//write a code which u want to perform at extreme end, after try and catch scenario, if u want to do something at end, then write code here. This is optional to write any code here.
}
```

#### Example: 
Handle Generic type of exception.
```apex
Try {
	string name;
	// name = 'Raj'; //we are expecting some data from end user but we didi not get and field is empty

	integer LengthOfText;
	LengthOfText = name. length () ;

	system.debug ('Length = ' + Length0fText );
}
catch (exception ex) {
	system.debug ('The info about error = ' + ex.getmessage()) ;
｝
finally{
	// signature work
}
```

```apex
// NullPointerException Example :

Try {
	string name;
	// name = 'Raj'; //we are expecting some data from end user but we didi not get and field is empty

	integer LengthOfText;
	LengthOfText = name. length () ;

	system.debug ('Length = ' + Length0fText );
}
catch (NullPointerException ex) {
	system.debug ('Seems you did not enter the needed data into salesforce') ;
｝
finally{
	// signature work
}
```

```apex
// DMLException Example :

try {
	lead l = new lead();
	insert l;
｝
catch(DMLException ex) {
	system.debug('Seems some DML error, we will take needed action!');
	//show end user a clear message: "seems record did not create/update properly.
}
finally {
	// signature work
}
```

```apex
// ListException Example :

try {
	List<Integer> MyList = new List<integer>();
	MyList.add(100);

	integer temp = MyList[1];
｝
catch(ListException ex) {
	system.debug('Seems some List error, we will take needed action!');
}
finally {
	// signature work
}
```

```apex
// QueryException Example :

try {
	lead l;
	l = [SELECT id FROM lead];
｝
catch(QueryException ex) {
	system.debug('Seems some Query error, we will take needed action!');
}
finally {
	// signature work
}
```

========== Ideal Code ==========
```apex
Try{
	// whatever logic or code you have..
	lead 1 = [SELECT id FROM lead 1;
｝
catch (ListException ex) {
	system.debug ('Seems some List error, we will take needed action!');
｝
catch (DMLException ex) {
	system.debug ('Seems some DMI error, we will take needed action!');
｝
catch (NullPointerException ex) {
	system.debug ('Seems some Null value processed!');
}
catch (QueryException ex) {
	system.debug ('Seems some Query error, we will t ake needed action!');
}
catch (Exception ex){
	//show generic error message to end user
}
finally{
	// signature work
	System.debug ('Control came to Finally block');
}
```

## Alternate of SELECT * in Salesforce

```apex
//Efficient way

//Declare object name
string VarTableName = 'Shekhar_Pen_c ';

//Create a string variable where u will have to store entire query
string VarQuery = 'SELECT ';

//Declare a list which can hold such multiple names of table
List<string> VarListOfTables = new List<string>();

//Add above table entry into Varlist0fTables collection
VarListOfTables.add(VarTableName);

//There is a very imp class "Schema" in SF which tells info about objects, fields etc
set<String> VarSetOfAllFields = schema.describeSObjects(VarListOfTables)[0].fields.getMap().keyset();
//above line will give u all the fields in that object

system.debug('The output =' + VarSetOfAllFields);
//means, list of all fields is ready

//use for-each loop to process 1 by 1 all fields present in VarSetOfAllFields
for(string VarOneFieldName : VarSetOfAllFields) {
	VarQuery = VarQuery + VarOneFieldName +', '
}

//current status of VarQuery = SELECT name,age_c,
VarQuery = VarQuery.removeEnd(', ');
//current status of VarQuery = SELECT name, age_c

//Add FROM and add table pame we decided above in the query
VarQuery = VarQuery + ' FROM ' + VarTableName;
//current status of VarQuery = SELECT name,age_c FROM Shekhar_Pen_c
```

## Javascript Remoting or Remoting or APEX Remoting or Remote Action
<img width="945" alt="Screenshot 2024-05-31 at 7 21 16 AM" src="https://github.com/therishabh/salesforce-apex/assets/7955435/3b6e26ea-f656-4395-8b9c-ae66baecc027">
<img width="934" alt="Screenshot 2024-05-31 at 7 21 54 AM" src="https://github.com/therishabh/salesforce-apex/assets/7955435/99ac13ca-c33b-40c2-b7c4-1eee5be15143">


<img width="956" alt="Screenshot 2024-05-31 at 7 20 58 AM" src="https://github.com/therishabh/salesforce-apex/assets/7955435/fee243ea-b43a-4a2c-a5d7-117546b22c52">



## Static Resources in Salesforce
Static resources in Salesforce are files that can be used in various parts of the platform, such as Visualforce pages, Apex classes, and Lightning components. These resources can include images, stylesheets, JavaScript files, and other types of documents. By using static resources, you can easily manage and reuse files across your organization, making it easier to maintain and update your Salesforce applications.

To create and manage static resources in Salesforce, you can use the Static Resource section of the Setup menu. Here, you can upload new resources, edit existing ones, and delete resources that are no longer needed.

To upload a new resource, click the "New" button, give your resource a name, select the file to upload and set the cache control. You can also upload multiple files at once by compressing them into a .zip file.

#### How To Create & Use Static Resources (Step by Step)
- Step 1: Navigate to Setup, search for "Static Resource" and click on "New" to create a new resource.
  ![image](https://github.com/therishabh/salesforce-apex/assets/7955435/1b8d24b8-6c89-4e3b-b895-4947dd4ef665)

- Step 2:- Enter a name, select a file to upload and set the cache control, then click "Save"
  ![image](https://github.com/therishabh/salesforce-apex/assets/7955435/cca845e1-920b-4a4a-bec7-b8c9a3d17f5f)

- Step 3:- Here you can see full detail about created static Resource and also see Where did it use, by clicking on where is this used?
  ![image](https://github.com/therishabh/salesforce-apex/assets/7955435/87f58bf3-7a30-400f-93e2-621f1320b4cc)

- Step 4 :- Now Referencing that Static Resource in Visualforce page ⇒ Open developer console ⇒ Create new Visualforce page
  ![image](https://github.com/therishabh/salesforce-apex/assets/7955435/d02bee42-1c2c-451e-be56-a942b71d9038)

- Step 5:- To render Visualforce page code ⇒ save the code and click on Preview button at the left corner on the top

#### Using Static Resources
##### Apex classes

In Apex classes, you can use the getContent() method to retrieve the contents of a static resource. The method returns the resource as a Blob, so you can then use it in various ways, such as creating a PDF document or sending it as an email attachment.

##### Lightning Web Component :

To use Static Resource in Lightning Web Component, First we need to import it from the import Salesforce_Images from '@salesforce/resourceUrl/SalesforceImages

Then, we can use Salesforce_Images to refer to the static resource.

## Generate PDF in Salesforce (Download or Send Email)

https://arrify.com/generate-pdf-in-salesforce/

## Security in Apex class
When you are writing code in Apex, the security of your code is of utmost importance. You should ensure that your code meets the latest Security guidelines.

### User Mode vs System Mode?
User Mode is when apex code is running by taking the user’s permissions and sharing into consideration. Profile permissions, Field level permissions, and sharing rules are respected when code is running in user mode.<br/><br/>
System Mode is when apex code is running without taking the user’s permissions and sharing into consideration. In system mode apex code has access to all objects, fields, and records and user’s sharing, FLS, and Profile permissions are not respected.

> Note : Visualforce page always works in User Mode but APEX class by default works in God / System Mode

```apex
public ******* class MyClass {
	//some code
｝
***** = we can use "with sharing" or "without sharing"
```

- without sharing => GOD / System Mode
- with sharing => User Mode

> If we dont write anything before "class" keyword, then also by default "without sharing" format will start
> If we dont want to allow APEX class behave like above scenario and we want to make that class more secure, then we need to use "with sharing" keyword.

Question : If by default keyword will be "without sharing" then why SF gave us that keyword?
Answer : We dont need "without sharing" keyword when we have standalone APEX class (one APEX class), but definitely we need "without sharing" keyword when we make class to class calls.

```apex
// Suppose, we have 2 below classes:
public with sharing class ClassA{
	// some code
	FunctionA () {
		// some code
		// ClassB.FunctionB() ;
	}
｝

public class ClassB {
	// some code
	FunctionB() {
		// This function contains a very general code which needs to be executed for all users, without checking any access
		// That's why developer of class B did not write any keyword for class B
	}
}
```

## Send Email in Salesforce with Apex Method


```apex
public class EmailManager {
    // Public method
    public void sendMail(String address, String subject, String body) {
        // Create an email message object
        Messaging.SingleEmailMessage mail = new Messaging.SingleEmailMessage();
        String[] toAddresses = new String[] {address};
        mail.setToAddresses(toAddresses);
        mail.setSubject(subject);
        mail.setPlainTextBody(body);
        // Pass this email message to the built-in sendEmail method 
        // of the Messaging class
        Messaging.SendEmailResult[] results = Messaging.sendEmail(
                                 new Messaging.SingleEmailMessage[] { mail });
        // Call a helper method to inspect the returned results
        inspectResults(results);
    }
    // Helper method
    private static Boolean inspectResults(Messaging.SendEmailResult[] results) {
        Boolean sendResult = true;
        // sendEmail returns an array of result objects.
        // Iterate through the list to inspect results. 
        // In this class, the methods send only one email, 
        // so we should have only one result.
        for (Messaging.SendEmailResult res : results) {
            if (res.isSuccess()) {
                System.debug('Email sent successfully');
            }
            else {
                sendResult = false;
                System.debug('The following errors occurred: ' + res.getErrors());                 
            }
        }
        return sendResult;
    }
}
```

> The inspectResults() helper method, which is called by sendMail(), writes messages to the log by using the System.debug() method to indicate whether the email send operation was successful or had errors.

#### Test
```apex
EmailManager em = new EmailManager();
em.sendMail('Your email address', 'Email Subject', 'Email Body');
```

## Server Side Validation

Types : 
- Client side validation (by Validation Rule)
- Server side validation (by Apex Class) (we can do in Trigger as well, but preference is to do this in Apex)

what is server side validation ?</br>
Checking the data in Apex and then throw error.

```apex
public class PenClassDemonstration{
    public static void applyDiscount(list<Pen__c> PensListNew) {
        for (Pen_c p :PensListNew){
            if(p.Price_c >= 100){
                p.Price_c = p.Price_c - 20 ;
            }
            else if(p.Price_c >= 70 && p.Price_c < 100 ){
                p.Price_c = p.Price_c - 10 ;
            ｝
            else if (p.Price_c >= 50 && p.Price_c < 70 ){
                p.Price_c = p.Price_c - 5 ;
            }
            else if (p.Price_c >= 20 && p.Price._c < 50 ){
                p.Price_c = p.Price_c-1 ;
            ｝
            else if （p.Price_c <=） ｛
                //p.addError('Please enter a valid price!'); -- at top
                p.Price_c.addError('Please enter a valid price!!');
            }
        }
    }
}
```

## Benefits of Test isRunningTest

There are many cases where we want to avoid some execution to happen on Test class level, which is fine if that works on APEX level (during non testing situation)

Test.isRunningTest() method is used to identify, if the piece of code being executed is invoked from a Test class execution or from other artefacts such as a Trigger, Batch Job etc

## Bulk API

#### Q. what is bulk API while working on data management or data loader ? <br/>
A. Bulk API is like a modified way to load large number of data in Salesforce

#### Q. Bulk api Works in synchronous mode or asynchronous mode ?</br>
A. The reason why it is able to load large number of Records because it actually works in asynchronous mode and these are the key reason why bulk API is able to manage large number of data.

#### Q. By default data loader which uses usage which API ? </br>
A. Data Loader by default is taking SOAP Based API.

#### Q. Have you ever enabled Buld API ?
A. YES (Then inteview again asked immediate question that have you seen any kind of difference in the behavious in your project while working with Buld API and when bulk api is not enabled)
A. Yes we have seen some difference between the data order with Bulk APi and non bulk API the major difference is by default you are not allowed to work on hard delete but after enable bulk api that Hard Delete option will be enabled. and normal way you have an option in setting to insert null data or not, but after enable bulk api that null data insert option will be disabled that means you can not insert null data.

![Screenshot 2024-05-31 at 12 35 11 PM](https://github.com/therishabh/salesforce-apex/assets/7955435/ded14278-7b4e-4f17-91a6-136f0dab2245)


Use Bulk API enabled means Bulk API + Parallel mode (Fast, but there may be chance to fail connection)<br/>
Use Enable serial mode for Bulk api means Bulk API + Serial Mode (slow, but connection will not fail because it will insert one by one record)

## Invocable Method
The invocable actions mechanism allows to create custom code and package it into components / actions available in declarative tools, such as Flow. You provide input values that the apex class does something with, and it gives you output values.

The InvocableMethod annotation tells Salesforce that this class should be exposed to users of invocable actions in the Flow Builder. It can be used to invoke a single Apex method.

**Sample Code**
```apex
public class AccountAction {
  @InvocableMethod(label='Get Account Names' description='Returns the list of account' category='Account')
  public static List<Account> getAccount(List<ID> ids) {
    // Do Something
  }
}
```
#### InvocableMethod Considerations:
- The invocable method must be static, public, or global, and its class must be an outer class.
- Only one method in a class can have the InvocableMethod annotation.
- Triggers can’t reference Invocable methods.
- No other annotations can be used alongside the InvocableMethod annotation.
- There can be at most one input parameter and its data type must be a list.

**Scenario:** </br>
Create a Custom Picklist field (status) on Account with Active, In Active & Hold. If the Account status is changed to In Active or Hold. Delete all the Closed Won Opportunity related to that Account.

- In Flow Screen firstly we need to select Record Triggered Flow
<img width="721" alt="Screenshot 2024-06-08 at 4 58 04 PM" src="https://github.com/therishabh/salesforce-apex/assets/7955435/bef72290-1b39-440d-b0bc-3aca137a510a">

<img width="721" alt="Screenshot 2024-06-08 at 4 58 52 PM" src="https://github.com/therishabh/salesforce-apex/assets/7955435/233d51cc-5a55-4214-b52b-8a5771ed51b1">
<img width="1439" alt="Screenshot 2024-06-08 at 4 59 59 PM" src="https://github.com/therishabh/salesforce-apex/assets/7955435/286f109b-c136-4078-9f7c-6523762a7a69">
<img width="722" alt="Screenshot 2024-06-08 at 5 00 18 PM" src="https://github.com/therishabh/salesforce-apex/assets/7955435/402cb36f-e1d7-49a5-a022-7f7f094f3825">

- now we need to call the apex class method, so for that we need to click on plus button in flow and then select Action option from list.
<img width="721" alt="Screenshot 2024-06-08 at 5 00 37 PM" src="https://github.com/therishabh/salesforce-apex/assets/7955435/5132a6fc-ddd2-44d1-aff6-b45580da7eac">

- now New Action screen will be open and in this screen we need to search label of out invocable Method.
<img width="721" alt="Screenshot 2024-06-08 at 5 04 29 PM" src="https://github.com/therishabh/salesforce-apex/assets/7955435/03f2500b-4833-42b7-8d5![Uploading Screenshot 2024-06-08 at 5.09.11 PM.png…]()
6-16dfc496d01c">

now everything is aligned but here is one problem, that is we are not checking if user change that status field only then after we need to run this flow
 <img width="1440" alt="Screenshot 2024-06-08 at 5 09 47 PM" src="https://github.com/therishabh/salesforce-apex/assets/7955435/b1d0e9cb-7ca6-4d49-9c95-fe3288c194b2">

## Check Limit in Salesforce

```apex
//Org limit checking code :

//Creating map for getting current limit of SF Org
Map<String, System.OrgLimit> VarOrgLimitMap = OrgLimits.getMap() ;

//Raw format output
System.debug ('VarOrgLimitMap-' + VarOrgLimitMap) ;

//Use below code to generate meaning / easy to comprehend data
for (String OrgLimitName :VarOrgLimitMap.keySet()) {

	//This provides data of used as well as remaining limits
	System.OrgLimit VarOrgLimit = VarOrgLimitMap.get(OrgLimitName) ;

	//This line will print information about names of all limits provided by this code
	System.debug ('Org Limit Name: ' + OrgLimitName) ;

	//This line gives the used limits in Org
	System.debug ('Org Limit Used:' + VarOrglimit.getValue()) ;

	//This line gives the user limits in Org
	System.debug('Org Limit Available:' + VarOrgLimit.getLimit());
}
```

## Platform Event
Platform Event is based on Event-Driven Architecture which enable apps to communicate inside and outside of Salesforce. Platform events are based on the publish/subscribe model and work directly with a message bus which handles the queue of incoming events and processes listening for them.

Platform Events are used to deliver notification within Salesforce or external app. It is based on Event-Driven Architecture. Platform event are helpful to overcome with point to point Salesforce Integration challenges.

#### Key Features of Platform Events
1. **Event-Driven Architecture:** Platform Events support an event-driven architecture, allowing different parts of a system to communicate with each other through events.</br>
2. **Publish-Subscribe Model:** Events are published by publishers and consumed by subscribers, following a publish-subscribe model.</br>
3. **Real-Time Processing:** Events can be processed in real-time, enabling immediate response to changes or actions within the system.</br>

```apex
public class OrderService {
    public void placeOrder(String orderId) {
        // Create instance of the event
        Order_Event__e orderEvent = new Order_Event__e();
        orderEvent.OrderID__c = orderId;

        // Publish the event
        Database.SaveResult sr = EventBus.publish(orderEvent);

        if (sr.isSuccess()) {
            System.debug('Event published successfully.');
        } else {
            System.debug('Event publish failed: ' + sr.getErrors()[0].getStatusCode() + ' - ' + sr.getErrors()[0].getMessage());
        }
    }
}
```

```apex
trigger OrderEventTrigger on Order_Event__e (after insert) {
    for (Order_Event__e event : Trigger.New) {
        // Process the event
        System.debug('Received Order Event: ' + event.OrderID__c);

        // Yahan pe hum custom logic add kar sakte hain jaise ki order processing
    }
}
```

```apex
// Example usage
OrderService orderService = new OrderService();
orderService.placeOrder('123456789012345678');
```

## Named Credentials
Named Credentials in Salesforce provide a secure and simplified way to configure and manage authentication details for external services. By using Named Credentials, you can avoid hard-coding sensitive information like usernames, passwords, and endpoint URLs in your Apex code. 

#### Key Features of Named Credentials
**1. Simplified Authentication:** Store authentication details in one place, reducing the need to manage credentials in your code.
**2. Security:** Keep sensitive information secure by storing it in Salesforce and avoiding exposure in code.
**3. Maintainability:** Easily update authentication details and endpoints without changing your code.

>> Use the callout: prefix followed by the Named Credential name to construct the endpoint URL. </br>
>> Example : string url  = 'callout:IMDB/imdb/imdbSearchByName?query='+searchText;

Named Credentials in Salesforce provide a robust, secure, and easy-to-maintain way to manage authentication details for external service integrations. By centralizing the storage of credentials and endpoint URLs, they enhance security and simplify the process of making API calls from Apex.


