## 📑 Table of Contents

* [How do you handle Governor Limits in large-scale applications?](#q-how-do-you-handle-governor-limits-in-large-scale-applications)
* [Difference between Future, Queueable, and Batch Apex](#q--difference-between-future-queueable-and-batch-apex)
* [How do you prevent recursive trigger execution?](#q-how-do-you-prevent-recursive-trigger-execution)
* [Difference between @wire and Imperative Apex](#q-difference-between-wire-and-imperative-apex)
* [How do you implement security in Salesforce?](#q-how-do-you-implement-security-in-salesforce)
* [Difference between Lookup vs Master-Detail (and When to use)](#q-difference-between-lookup-vs-master-detail-and-when-to-use)
* [When do you choose Flow over Apex Trigger?](#q-when-do-you-choose-flow-over-apex-trigger)
* [How do you optimize SOQL performance?](#q-how-do-you-optimize-soql-performance)
* [You have 1 million records to process. What approach?](#q-you-have-1-million-records-to-process-what-approach)
* [How do you handle code review as a Lead?](#q-how-do-you-handle-code-review-as-a-lead)
* [Record Locking Issue — How to Solve?](#q-record-locking-issue--how-to-solve)
* [How do you handle a production issue?](#q-how-do-you-handle-a-production-issue)
* [What is Serial Mode in Batch Apex?](#q-what-is-serial-mode-in-batch-apex)
* [What is Skinny Tables?](#q-what-is-skinny-tables)
* [What is Locking Query?](#q-what-is-locking-query)
* [Explain Retry Mechanism](#q-explain-retry-mechanism)
* [How do you design for Large Data Volume (LDV)?](#q-how-do-you-design-for-large-data-volume-ldv)
* [What is your Deployment Strategy?](#q-what-is-your-deployment-strategy)
* [What is your Branching Strategy?](#q-what-is-your-branching-strategy)
* [How do you mentor junior developers?](#q-how-do-you-mentor-junior-developers)
* [How do you design an Error Logging Framework in Salesforce?](#q-how-do-you-design-an-error-logging-framework-in-salesforce)
* [What is a Selective Query in Salesforce?](#q-what-is-a-selective-query-in-salesforce)
* [Why Test.startTest() and Test.stopTest() are important?](#q-why-teststarttest-and-teststoptest-are-important)
* [How do you design a scalable trigger framework?](#q-how-do-you-design-a-scalable-trigger-framework)
* [How do you handle 10k records update?](#q-how-do-you-handle-10k-records-update)
* [When to use Future vs Queueable vs Batch?](#q-when-to-use-future-vs-queueable-vs-batch)
* [What happens if a Queueable job fails?](#q-what-happens-if-a-queueable-job-fails)
* [Explain LWC lifecycle hooks](#q-explain-lwc-lifecycle-hooks)
* [Difference between @wire and Imperative Apex](#q-difference-between-wire-and-imperative-apex)
* [How do you optimize LWC performance?](#q-how-do-you-optimize-lwc-performance)
* [Difference between @wire and Imperative Apex (duplicate)](#q-difference-between-wire-and-imperative-apex)
* [How do you prevent recursive trigger execution?](#q-how-do-you-prevent-recursive-trigger-execution)
* [Difference between with sharing, without sharing, and inherited sharing](#q-difference-between-with-sharing-without-sharing-and-inherited-sharing)
* [Explain the complete Order of Execution in Salesforce](#q-explain-the-complete-order-of-execution-in-salesforce)
* [Difference between Custom Metadata vs Custom Settings](#q-difference-between-custom-metadata-vs-custom-settings)

---



# Q. How do you handle Governor Limits in large-scale applications?

### ✅ Detailed Explanation

Governor Limits are **runtime limits enforced by Salesforce** to ensure fair usage of shared resources in a multi-tenant environment.

---

### 🔹 Key Strategies to Handle Governor Limits

### 1. Bulkification (Most Important)

Always process **records in collections**, not single records.

❌ Bad

```apex
for(Account acc : Trigger.new){
    insert new Contact(AccountId = acc.Id);
}
```

✅ Good

```apex
List<Contact> contacts = new List<Contact>();
for(Account acc : Trigger.new){
    contacts.add(new Contact(AccountId = acc.Id));
}
insert contacts;
```

---

### 2. Avoid SOQL & DML inside loops

- Never write SOQL or DML inside a for loop
- Instead, collect data first, then perform one SOQL / one DML

❌ Wrong

```apex
for(Account acc : Trigger.new){
    Contact c = [SELECT Id FROM Contact WHERE AccountId = :acc.Id];
}
```

✅ Correct

* Collect IDs
* Query once
* Use Map

---

### 3. Use Collections (Maps & Sets) for Efficient Data Access
- Use Map and Set to store IDs and avoid duplicate queries
- Helps in reducing SOQL queries and improving performance

```apex
Map<Id, Account> accMap = new Map<Id, Account>(Trigger.new);
```

---

### 4. Use Asynchronous Processing

When data is heavy, move logic to async:

* Future
* Queueable
* Batch
* Platform Events

This helps because async has higher governor limits

---

### 5. Optimize SOQL Queries

- Query only required fields, not SELECT *

- Use WHERE conditions to limit records

- Use indexed fields when possible

---

### 6. Avoid Unnecessary Automation Layers

Too many flows + triggers = more limits consumption

👉 Follow:
**One Trigger per Object**
**Use Handler Classes**

---

### 7. Use Batch Apex for large data

- When records are in thousands or millions
- Batch processes data in chunks (200 records per batch)

---

### 8.Use Efficient Trigger Design

- One trigger per object
- Use Trigger Framework
- Move logic to handler classes

---

### **9. Optimize Data Volume**

* Use **Skinny tables**
* Archiving strategy
* Use async for large updates

---

### 🎯 Interview Key Points to Say

* Governor Limits ensure fair usage in multi-tenant architecture
* Bulkification is the most important design principle
* Avoid SOQL/DML inside loops
* Use Maps and Sets for efficient data handling
* Use async Apex (Future/Queueable/Batch) for heavy operations
* Follow single trigger and handler pattern
* Monitor limits using Limits class
* Reduce automation layers to avoid limit conflicts

---

# Q.  Difference between Future, Queueable, and Batch Apex

### ✅ Comparison Table

| Feature          | Future Apex           | Queueable Apex            | Batch Apex                    |
| ---------------- | --------------------- | ------------------------- | ----------------------------- |
| Execution Type   | Async                 | Async                     | Async (Chunk-based)           |
| Data Volume      | Small                 | Medium                    | Very Large (Millions)         |
| Parameter Types  | Only primitive        | Complex (Objects allowed) | Complex                       |
| Job Chaining     | ❌ Not supported       | ✅ Supported               | ✅ Supported                   |
| Monitoring       | ❌ No job ID           | ✅ Job ID available        | ✅ Full monitoring             |
| Use Case         | Simple async callouts | Complex async processing  | Large data processing         |
| Callouts Allowed | ✅ Yes                 | ✅ Yes                     | ✅ Yes                         |
| Execution Limit  | 50 per transaction    | 50 per transaction        | 5 concurrent batches          |
| Processing Style | Single transaction    | Single transaction        | Multiple chunks (200 records) |
| Best For         | Fire-and-forget       | Controlled async job      | Mass data handling            |

---

### 🧑‍💼 Real-Time Example

### Future Apex
- Used for send email
- Used for **simple callout** after record insert

### Queueable Apex

- Used for process order & call API
- Used when:
* Need chaining
* Need complex object passing
* Need job tracking

### Batch Apex
Used for:
* Updating 5 million records
* Data cleanup jobs
* Nightly processing

---

### 🎯 Interview Key Points to Say

* Future is simple async, only primitive parameters
* Queueable is more powerful, supports chaining and complex objects
* Batch Apex is for large data volumes in chunks
* Queueable is preferred over Future in modern Salesforce
* Batch Apex handles millions of records with chunk processing
* Queueable provides job monitoring via Job ID

---

### Code examples for **Future, Queueable, and Batch Apex** that you can easily remember

---

## 1. Future Method – Syntax

```apex
public class FutureExample {

    @future
    public static void updateAccounts(List<Id> accIds) {
        List<Account> accList = [SELECT Id, Name FROM Account WHERE Id IN :accIds];

        for(Account acc : accList) {
            acc.Name = acc.Name + ' Updated';
        }

        update accList;
    }
}
```

👉 **Calling Future Method**

```apex
FutureExample.updateAccounts(accIdList);
```

---

## 2. Queueable Apex – Syntax

```apex
public class QueueableExample implements Queueable {

    private List<Account> accList;

    // Constructor
    public QueueableExample(List<Account> accList) {
        this.accList = accList;
    }

    public void execute(QueueableContext context) {
        for(Account acc : accList) {
            acc.Name = acc.Name + ' Updated';
        }

        update accList;
    }
}
```

👉 **Calling Queueable Job**

```apex
List<Account> accList = [SELECT Id, Name FROM Account LIMIT 10];
System.enqueueJob(new QueueableExample(accList));
```

---

## 3. Batch Apex – Syntax

```apex
global class BatchExample implements Database.Batchable<sObject> {

    global Database.QueryLocator start(Database.BatchableContext bc) {
        return Database.getQueryLocator('SELECT Id, Name FROM Account');
    }

    global void execute(Database.BatchableContext bc, List<Account> scope) {

        for(Account acc : scope) {
            acc.Name = acc.Name + ' Updated';
        }

        update scope;
    }

    global void finish(Database.BatchableContext bc) {
        System.debug('Batch Job Completed');
    }
}
```

👉 **Calling Batch Job**

```apex
BatchExample batch = new BatchExample();
Database.executeBatch(batch, 200);
```

---


# Q. How do you prevent recursive trigger execution?

### Problem

Recursion happens when:

* Trigger updates a record
* That update again fires trigger
* Infinite loop occurs

---

### 🔹 Solution Approaches

### **1. Static Boolean Variable (Simple Approach)**

```apex
public class TriggerHelper {
    public static Boolean isFirstRun = true;
}
```

```apex
if(TriggerHelper.isFirstRun){
    TriggerHelper.isFirstRun = false;
    // logic here
}
```

---

### **2. Static Set of Record IDs (Better Approach)**

```apex
public class TriggerHelper {
    public static Set<Id> processedIds = new Set<Id>();
}
```

```apex
for(Account acc : Trigger.new){
    if(!TriggerHelper.processedIds.contains(acc.Id)){
        TriggerHelper.processedIds.add(acc.Id);
        // logic here
    }
}
```

---

### **3. Use Trigger Framework**

Use a **centralized handler class** that controls execution flow.

---

### **4. Use Field Change Check**

Run logic only if a field value changed:

```apex
if(acc.Status__c != Trigger.oldMap.get(acc.Id).Status__c){
    // run logic
}
```

---


### 🧑‍💼 Real-Time Example

We had a scenario:

* Account trigger updated Contact
* Contact trigger updated Account

👉 It created infinite loop

Solution:

* Used **static Set<Id> recursion guard**
* Added **field change condition**
* Moved logic to **service class**

---

### 🎯 Interview Key Points to Say

* Recursion occurs when trigger updates same object repeatedly
* Static Boolean or Static Set is used to control execution
* Best practice is using Static Set of processed record IDs
* Use field change checks to avoid unnecessary execution
* Use trigger framework to control logic flow
* Avoid updating same object unnecessarily inside trigger

---

# Q. Difference between @wire and Imperative Apex

### Detailed Explanation

In **LWC (Lightning Web Components)**, there are **two ways to call Apex**:

### 🔹 1. `@wire` (Reactive & Automatic)

* Used for **read-only operations (GET data)**
* Automatically executes when component loads or reactive parameter changes
* Managed by **LDS (Lightning Data Service)**
* Supports **caching**

### 🔹 2. Imperative Apex (Manual Call)

* Used for **create/update/delete or controlled fetch**
* Called **manually via JS function**
* No automatic execution
* More control over when and how to call

---

### 🔸 Difference Table

| Feature        | @wire                | Imperative Apex             |
| -------------- | -------------------- | --------------------------- |
| Execution      | Automatic            | Manual                      |
| Use Case       | Read-only data       | DML / controlled operations |
| Caching        | Yes (LDS cache)      | No (manual handling)        |
| Control        | Low                  | High                        |
| Error Handling | Auto handling        | Try-catch required          |
| Parameters     | Reactive             | Passed manually             |
| Re-execution   | Auto on param change | Manual re-call              |
| Best For       | UI data display      | Button click, form submit   |

---

### 🧑‍💼 Real-Time Example

* `@wire` → Load Account list on page load
* Imperative → On button click, create new Account

---

### 🎯 Interview Key Points

* `@wire` is reactive and used for read-only operations
* Imperative Apex gives full control and used for DML
* `@wire` supports caching via LDS
* Imperative is used for user-driven actions like button click
* Prefer `@wire` for performance and automatic refresh

---

### Code Example: 

## @wire Apex (Reactive & Automatic)

### ✅ Apex Class

```apex
public with sharing class AccountController {
    
    @AuraEnabled(cacheable=true)
    public static List<Account> getAccounts() {
        return [SELECT Id, Name FROM Account LIMIT 5];
    }
}
```

---

### ✅ LWC JS (using @wire)

```javascript
import { LightningElement, wire } from 'lwc';
import getAccounts from '@salesforce/apex/AccountController.getAccounts';

export default class WireExample extends LightningElement {

    @wire(getAccounts)
    accounts; // automatically gets data
}
```

---

### ✅ LWC HTML

```html
<template>
    <template if:true={accounts.data}>
        <template for:each={accounts.data} for:item="acc">
            <p key={acc.Id}>{acc.Name}</p>
        </template>
    </template>
</template>
```

---
---

## Imperative Apex (Manual Call)

### ✅ Apex Class (Same as above)

```apex
public with sharing class AccountController {
    
    @AuraEnabled
    public static List<Account> getAccountsImperative() {
        return [SELECT Id, Name FROM Account LIMIT 5];
    }
}
```

---

### ✅ LWC HTML

```html
<template>
    <lightning-button label="Load Accounts" onclick={handleClick}></lightning-button>

    <template for:each={accounts} for:item="acc">
        <p key={acc.Id}>{acc.Name}</p>
    </template>
</template>
```

---

### ✅ LWC JS (Imperative Call)

```javascript
import { LightningElement } from 'lwc';
import getAccountsImperative from '@salesforce/apex/AccountController.getAccountsImperative';

export default class ImperativeExample extends LightningElement {

    accounts = [];

    handleClick() {
        getAccountsImperative()
            .then(result => {
                this.accounts = result;
            })
            .catch(error => {
                console.error(error);
            });
    }
}
```

---

# Q. How do you implement security in Salesforce?

### ✅ Detailed Explanation

Salesforce security works on **4 layers**:

---

### 🔹 1. Organization Level Security

* Login IP range
* Login hours
* Password policies

---

### 🔹 2. Object Level Security (CRUD)

* Controlled via:

  * Profiles
  * Permission Sets

---

### 🔹 3. Field Level Security (FLS)

* Control access to specific fields
* Read / Edit permissions

---

### 🔹 4. Record Level Security

Controlled using:

* OWD (Org Wide Defaults)
* Role Hierarchy
* Sharing Rules
* Manual Sharing
* Apex Sharing

---

### 🔹 Apex Security

In code we enforce security using:

```apex
with sharing
Schema.sObjectType.Account.isAccessible()
Security.stripInaccessible()
```

---

### 🔹 LWC Security

* Use **@AuraEnabled(cacheable=true)** for read
* Use **Lightning Data Service**
* Respect FLS and sharing

---

### 🧑‍💼 Real-Time Example

In a banking project:

* Only Managers can see high-value accounts
* Implemented using:

  * Role hierarchy
  * Sharing rules
  * FLS on sensitive fields

---

### 🎯 Interview Key Points

* Salesforce security is based on **CRUD, FLS, and Sharing model**
* Use Profiles and Permission Sets for access control
* Use OWD + Role hierarchy + Sharing rules for record access
* Enforce security in Apex using **with sharing and stripInaccessible**
* Use Named Credentials and OAuth for integration security

---

# Q. Difference between Lookup vs Master-Detail (and When to use)

### 🔸 Key Differences

| Feature           | Lookup          | Master-Detail          |
| ----------------- | --------------- | ---------------------- |
| Relationship Type | Loose coupling  | Strong coupling        |
| Parent Required   | ❌ Optional      | ✅ Mandatory            |
| Child Ownership   | Independent     | Inherited from parent  |
| Sharing           | Separate        | Inherited              |
| Roll-up Summary   | ❌ Not available | ✅ Available            |
| Cascade Delete    | ❌ No            | ✅ Yes                  |
| Security          | Independent     | Controlled by parent   |
| Re-parenting      | Allowed         | Not allowed by default |

---

### 🧑‍💼 Real-Time Usage

### 🔹 Use Lookup when:

* Objects are loosely related
* Example: Contact ↔ Account (optional) (both looks lookup relationship but works like master detail because when any account will be delete then it's related contact will automatically delete.)

### 🔹 Use Master-Detail when:

* Strong dependency exists
* Example: Order → Order Line Items

---

### 🎯 Interview Key Points

* Lookup is loosely coupled relationship
* Master-detail is tightly coupled with parent dependency
* Master-detail supports roll-up summary and cascade delete
* Use Master-detail when child cannot exist without parent
* Use Lookup for flexible relationship

---

# Q. When do you choose Flow over Apex Trigger?

### ✅ Detailed Explanation

Salesforce now recommends **“Flow First Approach”**

Use **Flow** whenever possible, Apex only when needed.

---

### 🔹 Use Flow When:

* Simple business logic
* Field updates
* Email alerts
* Record creation
* Approval automation
* No complex logic

👉 Example:
When Account is created → create Contact

---

### 🔹 Use Apex Trigger When:

* Complex logic
* Cross-object transactions
* External integrations
* Performance critical operations
* Large data handling

👉 Example:
Update 5 related objects + call API

---

### 🔹 Comparison

| Feature         | Flow               | Apex Trigger        |
| --------------- | ------------------ | ------------------- |
| Complexity      | Low-Medium         | Medium-High         |
| Performance     | Moderate           | High                |
| Maintainability | High (declarative) | Developer dependent |
| Debugging       | UI based           | Code logs           |
| Bulk Handling   | Limited            | Strong              |

---

### 🧑‍💼 Real-Time Scenario

Project requirement:

* When Opportunity Closed Won
* Create Invoice
* Call external billing API
* Update 3 objects

👉 Solution:

* Flow used for simple updates
* Apex used for integration & complex logic

---

### 🎯 Interview Key Points

* Follow **Flow-first approach**
* Use Flow for simple automation
* Use Apex trigger for complex logic and integrations
* Apex provides better control and performance
* Combine Flow + Apex for scalable architecture

---

# Q. How do you optimize SOQL performance?

### ✅ Detailed Explanation

SOQL performance becomes critical when you are working with **large data volumes (LDV)** and **complex automation**.

The goal is to:

* Reduce **query time**
* Avoid **full table scans**
* Stay within **governor limits**
* Ensure **selectivity**

---

### 🔹 Key Techniques to Optimize SOQL

### 1. Use Selective Queries (Most Important)

A query is selective when it returns **<10% of records**.

👉 Use filters on:

* Indexed fields (Id, Name, External Id, Lookup fields)
* Custom indexed fields

```apex
SELECT Id FROM Account WHERE Status__c = 'Active'
```

---

### 2. Avoid SELECT *

Only fetch required fields.

❌ Bad

```apex
SELECT * FROM Account
```

✅ Good

```apex
SELECT Id, Name FROM Account
```

---

### 3. Avoid SOQL in Loops

Always bulkify queries using **Set + Map**

---

### 4. Use WHERE Conditions Properly

Avoid:

* `!=`
* `NOT IN`
* `LIKE '%text%'`

These break indexes.

---

### 5. Use Query Plan Tool

Salesforce provides **Query Plan Tool** to check:

* Cost
* Selectivity
* Index usage

---

### 6. Use LIMIT

If you don’t need all records:

```apex
SELECT Id FROM Account LIMIT 100
```

---

### 7. Use Skinny Tables (for LDV)

Salesforce Support can create **Skinny Tables** for performance.

---

### 8. Avoid Large OFFSET

Instead of OFFSET, use:

* Last record Id
* Pagination strategy

---

### 9. Use Relationship Queries Smartly

Avoid deep nested queries unnecessarily.

---

### 10. Use SOSL When Needed

Use SOSL when:

* Searching text across multiple objects

---

### 🧑‍💼 Real-Time Example

In one project:

* We had 5 million Accounts
* Query on non-indexed field → taking 20 sec

👉 Solution:

* Added **custom index**
* Optimized WHERE clause
* Reduced selected fields

Result:
- ⚡ Query time reduced to **< 1 sec**

---

### 🎯 Interview Key Points

* Always use **selective SOQL queries**
* Filter on **indexed fields**
* Avoid SOQL in loops
* Avoid negative filters like `!=`
* Use **Query Plan tool**
* Fetch only required fields
* Use Skinny tables for large data volume
* Use SOSL for text search

---

# Q. You have 1 million records to process. What approach?

### ✅ Detailed Explanation

Processing **1 million records** cannot be done in a single transaction.

👉 We must use **asynchronous, chunk-based processing**

---

### 🔹 Best Approach: **Batch Apex**

### Why Batch Apex?

* Processes data in **chunks (200 records per batch)**
* Each batch runs in **separate transaction**
* Resets governor limits each time

---

### 🔹 Batch Apex Structure

```apex
global class MyBatch implements Database.Batchable<sObject> {

    global Database.QueryLocator start(Database.BatchableContext bc){
        return Database.getQueryLocator('SELECT Id FROM Account');
    }

    global void execute(Database.BatchableContext bc, List<Account> scope){
        // process 200 records
    }

    global void finish(Database.BatchableContext bc){
        // post processing
    }
}
```

---

### 🔹 Additional Optimization Techniques

* Use **Database.Stateful** if needed
* Use **parallel execution**
* Use **bulkified logic**
* Use **Queueable chaining if needed**

---

### 🔹 Alternative Options

| Approach        | When to Use                  |
| --------------- | ---------------------------- |
| Batch Apex      | Best for millions of records |
| Bulk API        | External data load           |
| Platform Events | Event-driven processing      |
| Queueable       | Medium volume                |

---

### 🧑‍💼 Real-Time Example

Scenario:

* Update 1.2 million Contact records nightly

Solution:

* Batch Apex scheduled at night
* Batch size = 200
* Logging in custom object

Result:
- ✔ Completed in ~2 hours
- ✔ No governor limits error

---

### 🎯 Interview Key Points

* Use **Batch Apex for million records**
* Process data in **chunks of 200**
* Each batch runs in **separate transaction**
* Use **QueryLocator for large data**
* Use **parallel processing if possible**
* Monitor using **Async Apex Jobs**

---

# Q. How do you handle code review as a Lead?

### ✅ Detailed Explanation (Leadership Level)

As a Lead, code review is not just about syntax — it's about:

* Code quality
* Scalability
* Security
* Maintainability
* Best practices

---

### 🔹 My Code Review Approach

### **1. Follow Coding Standards**

Check:

* Naming conventions
* Clean structure
* Proper comments

---

### **2. Check Bulkification & Governor Limits**

* No SOQL/DML in loops
* Proper use of collections

---

### **3. Check Security**

* CRUD/FLS validation
* with sharing
* stripInaccessible()

---

### **4. Check Performance**

* Efficient SOQL
* Avoid unnecessary loops
* Async usage where needed

---

### **5. Check Design Pattern**

* Trigger framework
* Service layer
* Separation of concerns

---

### **6. Test Coverage & Quality**

* Minimum 75% coverage
* Positive + Negative + Bulk test cases

---

### **7. Error Handling & Logging**

* Try-catch blocks
* Proper error messages
* Logging framework

---

### **8. Provide Constructive Feedback**

* Give **clear suggestions**
* Explain **why change is needed**
* Encourage **best practices**

---

### **9. Knowledge Sharing**

* Conduct review sessions
* Document best practices

---

### 🧑‍💼 Real-Time Example

In my team:

* We follow **pull request review process**
* Each PR requires:

  * 1 senior approval
  * All test cases passing
* We use checklist:

  * Security ✔
  * Bulkification ✔
  * Limits ✔
  * Naming ✔

Result:
- ✔ High-quality code
- ✔ Less production bugs

---

### 🎯 Interview Key Points

* As a lead, I focus on **quality, performance, and scalability**
* I check **bulkification, SOQL optimization, and limits**
* I enforce **security (CRUD/FLS) and sharing rules**
* I ensure **proper design patterns and architecture**
* I review **test coverage and edge cases**
* I give **constructive feedback and mentor team members**
* I ensure **PR process and coding standards compliance**

---

# Q. Record Locking Issue — How to Solve?

### ✅ Problem

Record locking happens when:

* Two transactions try to update same record
* Salesforce locks the record

Error:
`UNABLE_TO_LOCK_ROW`

---

### 🔹 Common Scenarios

* Multiple triggers updating same parent record
* Batch jobs running in parallel
* Roll-up summary updates
* Integration updates same record

---

### 🔹 Solutions

### **1️⃣ Reduce Lock Contention**

Avoid updating same parent record multiple times

👉 Example:
Instead of updating Account in loop Collect all updates and update once

---

### **2️⃣ Use Proper Data Partitioning**

Divide data:

* Based on region
* Based on owner
* Based on date

---

### **3️⃣ Avoid Parallel Processing**

Batch Apex:

```apex
Database.executeBatch(batch, 200);
```

Use **serial mode** if locking issue:

```apex
Database.executeBatch(batch, 200, true);
```

---

### **4️⃣ Use FOR UPDATE Carefully**

Use locking query only when necessary

---

### **5️⃣ Retry Mechanism**

If lock occurs:

* Retry after delay
* Use Queueable chaining

---

### **6️⃣ Reduce Roll-up Summary Dependencies**

Because roll-ups lock parent record

---

### **7️⃣ Use Asynchronous Updates**

Move logic to:

* Queueable Apex
* Platform Event

---

### 🧑‍💼 Real-Time Example

Scenario:

* Opportunity updates Account Total Revenue
* Multiple users updating Opportunities simultaneously

👉 Caused Account record locking

Solution:

* Removed direct Account update
* Used **Queueable job to aggregate and update once**

---

### 🎯 Interview Key Points

* Record locking occurs due to concurrent updates
* I reduce locking by **minimizing parent record updates**
* I use **data partitioning strategy**
* I avoid **parallel batch execution**
* I use **async processing to reduce contention**
* I implement **retry mechanism for failed updates**

---

# Q. How do you handle a production issue?

### ✅ Detailed Approach (Leadership + Process)

As a Lead, handling production issue requires **structured approach**.

---

### 🔹 My Approach (Step-by-Step)

### **1️⃣ Identify & Understand Issue**

* Get error logs
* Check debug logs
* Understand impact

---

### **2️⃣ Categorize Severity**

| Severity | Example             |
| -------- | ------------------- |
| High     | System down         |
| Medium   | Feature not working |
| Low      | UI issue            |

---

### **3️⃣ Immediate Fix (Hotfix)**

* Disable automation (if needed)
* Create quick patch
* Deploy via **quick deployment**

---

### **4️⃣ Root Cause Analysis**

* Check logs
* Check recent deployments
* Reproduce issue in sandbox

---

### **5️⃣ Permanent Fix**

* Code fix
* Add validation
* Add test cases

---

### **6️⃣ Testing**

* Unit testing
* UAT validation

---

### **7️⃣ Deploy & Monitor**

* Deploy fix
* Monitor logs
* Confirm with users

---

### **8️⃣ Documentation & Learning**

* Document RCA
* Update knowledge base
* Improve process

---

### 🧑‍💼 Real-Time Example

Production issue:

* Invoice creation failing

Steps I took:

1. Checked logs → found null pointer
2. Disabled trigger temporarily
3. Fixed code with null check
4. Deployed hotfix
5. Added test case
6. Updated documentation

---

### 🎯 Interview Key Points

* I follow structured **incident management process**
* First I **analyze logs and identify impact**
* I implement **quick hotfix to stabilize system**
* Then perform **root cause analysis**
* Implement **permanent fix with test coverage**
* Deploy and **monitor system**
* Document learnings and improve process

---

# Q. What is Serial Mode in Batch Apex?

### ✅ Detailed Explanation

By default, **Batch Apex runs in parallel mode**, meaning:

* Multiple batch chunks (scope = 200 records) run **simultaneously**

This improves performance, but it can cause:
- ❌ Record locking issues
- ❌ Data conflicts

---

### 🔹 Serial Mode

👉 **Serial Mode ka matlab hai:**

> Batch Apex ke saare batches **ek ke baad ek (sequence me)** execute honge, **parallel me nahi**.

Matlab:

* Pehla batch complete hoga
* Fir dusra start hoga
* Fir teesra…
  ➡️ **Ek time par sirf ek hi batch execute hoga**

---

### 🔹 Normally kya hota hai? (Default Behavior)

Salesforce Batch Apex by default **Parallel Mode** me run karta hai:

* Multiple batches **same time pe run** ho sakte hain
* Fast hota hai
* But **data conflict (record locking)** ka risk hota hai

---

### 🔹 Serial Mode kyun use karte hain?

> 👉 Jab hume **data consistency** chahiye aur **record locking issue avoid karna hai**

---

**Serial mode means batch jobs run one chunk at a time (sequentially)** instead of parallel.

> 👉 Only **one batch execution runs at a time**

---

### 🔸 How to Enable Serial Mode

```apex
Database.executeBatch(new MyBatchClass(), 200, true);
```

👉 The third parameter `true` = **serial mode**

---

### 🎯 Interview Key Points

* Serial mode executes batch jobs **sequentially**
* Prevents **record locking issues**
* Slower than parallel but **safer for data consistency**
* Used when **multiple records update same parent**

---

# Q. What is Skinny Tables?

### ✅ Detailed Explanation

**Skinny Tables** are special **Salesforce-managed database tables** created by Salesforce Support to **improve query performance**.

They contain:

* Frequently used fields
* From one or multiple objects

---

### 🔹 Why Skinny Tables?

In large orgs with **millions of records**, standard tables are heavy.

👉 Skinny tables:

* Reduce joins
* Reduce table size
* Improve query speed

---

### 🔹 Key Features

* Created by **Salesforce Support only**
* Contains **subset of fields**
* Supports **standard & custom fields**
* Used for **read-heavy operations**

---

### 🧑‍💼 Real-Time Example

Scenario:

* Account object with **5 million records**
* Query joining multiple fields taking **10+ seconds**

Solution:
✔ Created Skinny table with:

* Id
* Name
* Status
* Region

Result:
⚡ Query time reduced to **< 1 second**

---

### ⚠️ Limitations

* No formula fields
* No long text
* Not automatically updated for schema changes
* Cannot include all field types

---

### 🎯 Interview Key Points

* Skinny tables improve **SOQL performance in large data volume**
* Created by **Salesforce support**
* Contain **subset of frequently used fields**
* Reduce joins and improve speed
* Used in **LDV (Large Data Volume) orgs**

---

# Q. What is Locking Query?

### ✅ Detailed Explanation

A **locking query** is a SOQL query with `FOR UPDATE` clause that **locks records** during transaction.

```apex
SELECT Id, Name FROM Account WHERE Id = :accId FOR UPDATE
```

---

### 🔹 What happens?

* Record gets **locked**
* Other transactions **cannot update it**
* Prevents **data inconsistency**

---

### 🔹 When to Use?

Use when:

* You need **data consistency**
* You are doing **financial calculations**
* You want to avoid **race conditions**

---

### 🧑‍💼 Real-Time Example

Scenario:

* Two users updating same **bank balance**

Without lock:
❌ Data inconsistency

With lock:
✔ One transaction waits → data safe

---

### ⚠️ Caution

* Overuse can cause **performance issues**
* Can increase **lock contention**

---

### 🎯 Interview Key Points

* Locking query uses `FOR UPDATE`
* Locks record during transaction
* Prevents **concurrent update conflicts**
* Used in **critical data scenarios**
* Should be used **carefully to avoid performance issues**

---

# Q. Explain Retry Mechanism

### ✅ Detailed Explanation

A **Retry Mechanism** is used when:

* An operation fails due to **temporary issue**
* Example:

  * Record locking
  * API timeout
  * Network failure

Instead of failing permanently, system **retries the operation**

---

### 🔹 Why Retry?

Because many failures are:

* Temporary
* Resolvable on retry

---

### 🔹 How to Implement in Salesforce

### **Approach 1: Queueable Retry**

```apex
try{
   // logic
}catch(Exception e){
   System.enqueueJob(new RetryJob());
}
```

---

### **Approach 2: Retry with Counter**

```apex
if(retryCount < 3){
   retryCount++;
   enqueue again
}
```

---

### **Approach 3: Custom Retry Queue Object**

Store failed records in object:

* Record Id
* Error
* Retry count
* Status

Then process using:

* Batch Apex
* Scheduled Job

---

### **Approach 4: Middleware Retry**

If using MuleSoft:

* Retry handled in middleware layer

---

### 🧑‍💼 Real-Time Example

Scenario:

* Integration callout failed due to timeout

Solution:

* Store failed record in **Retry Queue**
* Scheduled batch retries every 30 mins
* Max 3 retries

Result:
✔ 98% success after retry

---

### 🎯 Interview Key Points

* Retry mechanism handles **temporary failures**
* Used for **integration, locking, async jobs**
* Implement using **Queueable, Batch, or Scheduled jobs**
* Maintain **retry count and error logs**
* Improves system **resilience and reliability**

---

# Q. How do you design for Large Data Volume (LDV)?

### ✅ Detailed Explanation

LDV means:
👉 Millions of records in objects like Account, Opportunity, Case

In LDV systems, main challenges are:

* Slow queries
* Record locking
* Governor limits
* Reporting performance

---

### 🔹 LDV Design Principles

### **1. Use Selective SOQL Queries**

* Filter on indexed fields
* Avoid full table scan

---

### **2. Data Archiving Strategy**

* Move old data to:

  * Big Objects
  * External storage
* Keep active data small

---

### **3. Use Skinny Tables**

* Improve query performance
* Reduce joins

---

### **4. Proper Data Model Design**

* Avoid too many lookup relationships
* Use denormalized data where required

---

### **5. Use Asynchronous Processing**

* Batch Apex
* Queueable
* Platform Events

---

### **6. Avoid Record Locking**

* Reduce parent record updates
* Use serial mode where required

---

### **7. Use Partitioning Strategy**

Split data based on:

* Region
* Business unit
* Date

---

### **8. Use Custom Indexes**

For frequently filtered fields

---

### **9. Reporting Optimization**

* Use custom report types
* Avoid too many joins

---

### **10. Integration Optimization**

* Use Bulk API for large data
* Use CDC instead of polling

---

### 🧑‍💼 Real-Time Example

In a telecom project:

* 10 million Case records

Solution:

* Archive old cases to Big Objects
* Added indexes on Status & Region
* Used Skinny tables
* Batch jobs for processing

Result:
⚡ Report time reduced from 30 sec → 3 sec

---

### 🎯 Interview Key Points

* Use **selective queries and indexed fields**
* Implement **data archiving strategy**
* Use **Skinny tables and custom indexes**
* Use **Batch Apex and async processing**
* Avoid record locking using proper design
* Partition data and optimize reports

---

# Q. What is your Deployment Strategy?

### ✅ Detailed Explanation

A good deployment strategy ensures:

* Safe releases
* Minimal downtime
* Easy rollback

---

### 🔹 My Deployment Approach

### **1. Use CI/CD Pipeline**

Tools:

* GitLab CI
* Jenkins
* GitHub Actions

---

### **2. Environment Strategy**

```
Dev → QA → UAT → Pre-Prod → Production
```

---

### **3. Use Unlocked Packages / Metadata API**

* Modular deployments
* Easy rollback

---

### **4. Automated Testing**

* Apex tests
* UI tests (Cypress)
* Code coverage > 75%

---

### **5. Use Feature Toggles**

Deploy code but enable later

---

### **6. Perform Smoke Testing**

After deployment

---

### **7. Rollback Plan**

* Backup metadata
* Revert deployment quickly

---

### **8. Zero Downtime Strategy**

* Deploy during low usage time
* Use async operations

---

### 🧑‍💼 Real-Time Example

In my project:

* Used GitLab CI pipeline
* Automatic deployment to QA
* Manual approval for production
* Smoke testing post deploy

---

### 🎯 Interview Key Points

* I use **CI/CD pipeline for deployment**
* Follow **multi-environment strategy**
* Ensure **automated testing and validation**
* Use **feature toggles and rollback plan**
* Perform **post-deployment smoke testing**

---

# Q. What is your Branching Strategy?

### ✅ Detailed Explanation

Branching strategy helps manage:

* Multiple developers
* Multiple features
* Stable releases

---

### 🔹 My Preferred Strategy: **Git Flow**

### 🔹 Branches

| Branch      | Purpose            |
| ----------- | ------------------ |
| main/master | Production code    |
| develop     | Integration branch |
| feature/*   | New feature        |
| release/*   | Pre-release        |
| hotfix/*    | Production fixes   |

---

### 🔹 Flow

```
feature → develop → release → main
```

Hotfix:

```
hotfix → main → develop
```

---

### 🔹 Benefits

* Clean version control
* Easy rollback
* Parallel development

---

### 🧑‍💼 Real-Time Example

* Developers work on feature branches
* Merge request → code review
* Merge to develop → QA testing
* Release branch → UAT → production

---

### 🎯 Interview Key Points

* I follow **Git Flow branching strategy**
* Use **feature branches for development**
* Use **release branch for UAT testing**
* Use **hotfix branch for urgent production fixes**
* Ensure **code review before merge**

---

# Q. How do you mentor junior developers?

### ✅ Detailed Explanation (Leadership)

Mentoring is not just about teaching coding — it’s about:

* Building problem-solving mindset
* Improving code quality
* Growing confidence

---

### 🔹 My Mentoring Approach

### **1. Knowledge Sharing Sessions**

* Weekly sessions
* Topics like Apex, LWC, Integration

---

### **2. Pair Programming**

* Work together on complex features

---

### **3. Code Reviews with Feedback**

* Explain why improvement is needed
* Share best practices

---

### **4. Provide Learning Path**

* Give roadmap:

  * Apex → LWC → Integration → Architecture

---

### **5. Encourage Best Practices**

* Bulkification
* Security
* Design patterns

---

### **6. Give Ownership**

* Assign small modules
* Build confidence

---

### **7. Continuous Feedback**

* Appreciate good work
* Suggest improvements

---

### **8. Real-World Problem Solving**

* Give real scenarios to solve

---

### 🧑‍💼 Real-Time Example

In my team:

* I mentored 3 junior devs
* Conducted weekly Apex sessions
* Helped them build trigger framework

Result:
✔ They became independent developers

---

## 🎯 Interview Key Points

* I mentor through **code reviews, pair programming, and knowledge sessions**
* I provide **clear learning roadmap**
* I focus on **best practices and architecture thinking**
* I give **ownership to build confidence**
* I provide **continuous feedback and support**

---


















# Q: How do you avoid hardcoding?

#### Answer (Interview-ready, balanced)

To avoid hardcoding in Salesforce, I make my code **configurable and dynamic** instead of using fixed values.

#### 🔸 Main approaches I use:

1. **Custom Metadata Types (best approach)**

   * Store business logic values like status, type, mappings
   * Admin can change values without code deployment
     👉 Example: Customer Type → Rating mapping

2. **Custom Labels**

   * For messages, error text, UI labels
     👉 Example: error message or button text

3. **Named Credentials**

   * For API endpoints and authentication
     👉 Avoid hardcoding URLs or credentials

4. **Avoid hardcoded IDs**

   * Instead of writing RecordTypeId or UserId directly, fetch dynamically using SOQL or Schema methods

5. **Constants Class (for fixed values)**

   * For values that never change like status codes

---

#### 🎯 Small Example

❌ Hardcoded:

```apex
if(acc.Type == 'Customer')
```

✔ Better:

```apex
if(acc.Type == Label.Customer_Type)
```

or use Custom Metadata.

---

#### 🧠 One-line interview summary you can speak

> “To avoid hardcoding, I use Custom Metadata for business configurations, Custom Labels for text, Named Credentials for endpoints, and dynamic queries for IDs. This makes the system flexible and avoids code changes for small business updates.”

---
---
---
---
---

# Q: How do you ensure code quality in Salesforce?

#### 🎯 Short Intro (you can start like this)

> “To ensure code quality, I follow proper coding standards, use best practices, write strong test classes, and use code reviews and static analysis tools.”

---

### 🔹 Key ways to ensure code quality

#### 1. Follow Coding Standards & Best Practices

* Use proper naming conventions
* Follow **one trigger per object**
* Use **trigger framework**
* Write **bulk-safe code** (no SOQL/DML in loops)

👉 This makes code clean and scalable

---

#### 2. Write Strong Test Classes

* Maintain **at least 75% coverage (but aim 90%+)**
* Cover **positive + negative + edge cases**
* Use **Test.startTest() / stopTest()**
* Use **mocking for callouts**

👉 Ensures code works correctly in all scenarios

---

#### 3. Code Review Process

* Every code goes through **peer review**
* Check for:

  * logic correctness
  * performance issues
  * security issues
  * governor limits

👉 Helps catch bugs early

---

#### 4. Use Static Code Analysis Tools

Tools like:

* PMD
* SonarQube
* CodeScan

👉 Detect issues like:

* SOQL in loop
* unused variables
* bad practices

---

#### 5. Follow Security Best Practices

* Use **with sharing / inherited sharing**
* Enforce **FLS and CRUD** using `stripInaccessible`
* Avoid exposing sensitive data

---

#### 6. Proper Error Handling & Logging

* Use try-catch
* Use centralized **error logging framework**
* Avoid exposing raw errors to users

---

#### 7. Performance Optimization

* Use collections and maps
* Avoid unnecessary queries
* Use async processing for heavy jobs

---

#### 8. Version Control & CI/CD

* Use Git (GitHub/GitLab)
* Use proper branching strategy
* Use CI pipeline for automated testing

---

#### 🎯 Small Real-time example

> “In my project, we used a trigger framework, code reviews, and PMD static analysis. We also ensured 90%+ test coverage and proper bulk handling. This helped us reduce production issues significantly.”

---

#### 🧠 One-line strong interview summary

> “I ensure code quality by following coding standards, writing bulk-safe and testable code, performing peer code reviews, using static analysis tools, and ensuring proper test coverage and security checks.”

---
---
---
---
---


# Q. How do you design an Error Logging Framework in Salesforce?

#### 🧠 First understand the problem

In real projects, errors can happen anywhere:

* Trigger fails
* Batch job fails
* Integration API fails
* LWC throws exception
* Validation fails unexpectedly

👉 If we **don’t log errors properly**, then:

- ❌ We don’t know what failed
- ❌ We can’t debug production issues
- ❌ We can’t retry failed integrations

So we need a **centralized error logging framework**

---

#### 🎯 Goal of Error Logging Framework

The framework should:

- ✔ Capture all errors from any layer
- ✔ Store error details in one place
- ✔ Help debugging
- ✔ Allow retry (if needed)
- ✔ Send alert (optional)

---

#### 🧱 Step 1: Create Custom Object for Logging

Create a custom object like:

#### 🔹 Object: `Error_Log__c`

#### 🔹 Important Fields

| Field            | Purpose                            |
| ---------------- | ---------------------------------- |
| Name             | Auto number                        |
| Error_Message__c | Actual exception message           |
| Stack_Trace__c   | Full stack trace                   |
| Class_Name__c    | Which class error occurred         |
| Method_Name__c   | Which method                       |
| Record_Id__c     | Related record                     |
| User_Id__c       | Who triggered error                |
| Status__c        | New / Resolved / Retry             |
| Payload__c       | (For integration request/response) |

---

#### 🧠 Step 2: Create Central Logging Utility Class

Create one reusable Apex class:

#### 🔹 `ErrorLogger.cls`

```apex
public class ErrorLogger {

    public static void logError(Exception e, String className, String methodName, Id recordId) {

        Error_Log__c log = new Error_Log__c();
        log.Error_Message__c = e.getMessage();
        log.Stack_Trace__c = e.getStackTraceString();
        log.Class_Name__c = className;
        log.Method_Name__c = methodName;
        log.Record_Id__c = recordId;
        log.User_Id__c = UserInfo.getUserId();

        insert log;
    }
}
```

---

#### 🧠 Step 3: Use Logger in Code (Triggers / Classes / Batch)

Whenever you write logic:

#### 🔹 Example in Trigger Handler

```apex
try {
   // business logic
} catch(Exception e) {
   ErrorLogger.logError(e, 'AccountService', 'createContact', acc.Id);
}
```

---

#### 🧠 Step 4: Logging in Batch / Queueable

### Example:

```apex
global void execute(Database.BatchableContext bc, List<Account> scope){
    try {
        // logic
    } catch(Exception e){
        ErrorLogger.logError(e, 'AccountBatch', 'execute', null);
    }
}
```

---

#### 🧠 Step 5: Logging in Integration Callouts

```apex
try {
    HttpResponse res = http.send(req);

    if(res.getStatusCode() != 200){
        throw new CalloutException('API failed');
    }

} catch(Exception e){
    ErrorLogger.logError(e, 'PaymentIntegration', 'sendData', recordId);
}
```

---

#### 🧠 Step 6: Add Retry Mechanism (Advanced)

For integrations:

👉 Add field `Retry_Count__c`

If failed:

* Admin can click “Retry”
* Batch job can retry failed logs

---

#### 🧠 Step 7: Optional Features (Advanced Framework)

You can enhance framework with:

### 🔹 1. Platform Events

* Publish error event
* External system or monitoring tool listens

### 🔹 2. Email Notification

* Send email to admin on critical errors

### 🔹 3. Custom Metadata to control logging

* Turn ON/OFF logging

### 🔹 4. Log Levels

Like:

* INFO
* DEBUG
* ERROR
* FATAL

---

#### 🎯 Real-time Project Example (Very Important for Interview)

“In my project, we had multiple integrations like Payment API and Shipping API. Sometimes API failed due to timeout or invalid response. We implemented an Error Logging Framework using a custom object Error_Log__c where we stored request payload, response, error message, and record Id. We also created a retry batch job which picked failed logs and retried the callout.”

---

#### 🎯 Best Practices

- ✔ Never expose raw error to user
- ✔ Always use try-catch
- ✔ Log meaningful message
- ✔ Avoid logging inside loops
- ✔ Use bulk-safe logging
- ✔ Store payload for integrations

---

#### ❗ Important Interview Follow-up Questions

Interviewer may ask:

👉 Where will you store logs?</br>
✔ Custom Object

👉 How will you handle bulk errors?</br>
✔ Use list of Error_Log__c and insert once

👉 How will you retry failed integrations?</br>
✔ Use Batch job reading failed logs

👉 How will you monitor logs?</br>
✔ Report / Dashboard / Email alerts

👉 Will logging affect governor limits?</br>
✔ Yes → so bulk insert logs

---

#### 🎤 How you should answer in Interview (Speak this)

You can say like this:

> “I design error logging framework using a centralized utility class and a custom object to store all error details like message, stack trace, class name, and related record. I wrap business logic in try-catch and log errors using a reusable method. For integrations, I also store payload and implement retry mechanism using batch job. This helps in monitoring, debugging, and retrying failures in production.”

---
---
---
---
---

# Q. What is a Selective Query in Salesforce?

#### 🔹 Answer (short & interview-ready)

A **selective query** is a SOQL query that uses **indexed fields and filters data efficiently**, so Salesforce can return results **quickly without scanning the whole table**.

👉 It is important for **large data volumes (LDV)** and to avoid **query timeouts or non-selective query errors**.

---

#### 🔹 Simple Example

❌ **Non-selective query** (slow)

```apex
SELECT Id, Name FROM Account WHERE Name LIKE '%Tech%'
```

✔ **Selective query** (fast)

```apex
SELECT Id, Name FROM Account WHERE Id = :accId
```

or

```apex
SELECT Id FROM Account WHERE External_Id__c = :extId
```

(External Id is indexed)

---

#### 🔹 How to make a query selective

* Filter on **indexed fields** (Id, Name, External Id, Unique fields, etc.)
* Use **highly selective conditions** (return small % of records)
* Avoid `LIKE '%value%'` and filters on non-indexed fields
* Use **AND conditions** to narrow down results

---

#### 🎯 One-line interview answer

> “A selective query is a SOQL query that uses indexed fields and highly selective filters so it returns results efficiently, especially in large data volumes.”

---
---
---
---
---

# Q. Why `Test.startTest()` and `Test.stopTest()` are important?

#### 🔹 Short, interview-ready answer

* **`Test.startTest()`** is used to **reset governor limits** and mark the start of the actual code you want to test.
* **`Test.stopTest()`** is used to **execute all asynchronous code** like **future, queueable, batch, and scheduled jobs**.

---

#### 🔹 Why we use them

1. **Fresh limits for main logic**

   * Setup code may use limits
   * `startTest()` gives fresh limits to your actual method

2. **Execute async code**

   * Async jobs do not run in test until `stopTest()` is called

---

#### 🔹 Small example

```apex
Test.startTest();

System.enqueueJob(new MyQueueable());

Test.stopTest(); // queueable executes here
```

---

#### 🎯 One-line answer

> “We use Test.startTest() to reset limits and Test.stopTest() to execute asynchronous code so we can properly test the result.”

---
---
---
---
---
# Q. How do you design a scalable trigger framework?

#### 🔹 Short, interview-ready answer

To make triggers scalable and maintainable, I follow a **trigger framework pattern** where I **separate logic from the trigger** and make it **bulk-safe and reusable**.

---

#### 🔹 Key design points

* **One trigger per object**
  → Avoid conflicts and maintain control

* **Keep trigger very thin**
  → Only route to handler methods (no business logic inside trigger)

* **Use a Handler class**
  → Separate methods for each context like `beforeInsert`, `afterUpdate`

* **Bulk-safe logic**
  → Use Lists/Maps, no SOQL or DML inside loops

* **Use Service layer**
  → Keep business logic reusable and testable

* **Handle recursion**
  → Use static Set/flag to prevent duplicate execution

* **Control execution using Custom Metadata (optional)**
  → Enable/disable logic without code change

---

#### 🔹 Simple structure

```
Trigger
  → Handler Class
      → Service Class
```

---

#### 🔹 Small example

```apex
trigger AccountTrigger on Account (before insert, after insert) {
    AccountTriggerHandler handler = new AccountTriggerHandler();

    if(Trigger.isBefore && Trigger.isInsert){
        handler.beforeInsert(Trigger.new);
    }
}
```

---

#### 🎯 One-line interview answer

> “I design a scalable trigger framework by keeping one trigger per object, moving logic to handler and service classes, making it bulk-safe, and using a structured pattern to make the code reusable and easy to maintain.”

---
---
---
---
---

# Q. How do you handle 10k records update?

#### 🔹 Short, interview-ready answer

For large data updates like **10k+ records**, I use **Batch Apex** so that records are processed in **small chunks** and **governor limits don’t get hit**.

---

#### 🔹 Main approach

* Use **Batch Apex**
* Process records in **batches (e.g., 200 per batch)**
* Limits reset for each batch execution

---

#### 🔹 Simple example

```apex
Database.executeBatch(new MyBatchClass(), 200);
```

👉 Each batch handles only 200 records → safe from limits

---

#### 🔹 When to use other options

* **Queueable Apex** → for medium volume (few thousands)
* **Data Loader / Bulk API** → for data migration or external loads

---

#### 🎯 One-line interview answer

> “To handle 10k records, I use Batch Apex so records are processed in chunks and governor limits are managed efficiently.”

---
---
---
---
---

# Q. When to use Future vs Queueable vs Batch?

#### 🔹 Short, interview-ready answer

All three are **asynchronous Apex**, but used for different data volumes and complexity.

---

#### 🔹 When to use each

**1. Future Method (`@future`)**

* Use for **small, simple background tasks**
* Example: send email, simple callout
* Limitations: no chaining, no complex objects

---

**2. Queueable Apex**

* Use for **medium complexity tasks**
* Can pass **complex objects**
* Supports **chaining (one job after another)**
* Better monitoring than future

---

**3. Batch Apex**

* Use for **large data volume (1000+ or 10k+ records)**
* Processes data in **chunks (batches)**
* Best for **scheduled or bulk processing**

---

#### 🔹 Simple memory trick

* **Small task → Future**
* **Medium task → Queueable**
* **Large data → Batch**

---

#### 🎯 One-line interview answer

> “I use Future for small async tasks, Queueable for medium and complex processing with chaining, and Batch Apex for large data volumes where records need to be processed in chunks.”

---
---
---
---
---

# Q. What happens if a Queueable job fails?

#### 🔹 Short, interview-ready answer

* If a Queueable job fails due to an exception, its **status becomes “Failed”** in **Apex Jobs**.
* The **error message and stack trace** are logged, and **no further chained jobs execute**.

---

#### 🔹 How we handle it (best practice)

* Use **try–catch** inside `execute()` to handle errors
* **Log the error** (custom log object / logging framework)
* **Retry the job** (enqueue again or use a scheduled/batch retry)
* Optionally **notify admin** (email/alert)

---

#### 🔹 Small example

```apex
public class MyQueueable implements Queueable {
    public void execute(QueueableContext qc){
        try{
            // logic
        } catch(Exception e){
            ErrorLogger.logError(e, 'MyQueueable', 'execute', null);
        }
    }
}
```

---

#### 🎯 One-line interview answer

> “If a Queueable job fails, its status becomes Failed in Apex Jobs, the error is logged, and any chained jobs don’t run. We handle it using try-catch, logging, and retry mechanisms.”

---
---
---
---
---

# Q. Explain LWC lifecycle hooks

#### 🔹 Short, interview-ready answer

LWC lifecycle hooks are **methods that run at different stages of a component’s life** — from creation to removal.

---

#### 🔹 Main lifecycle hooks

**1. constructor()**

* Called when component is created
* Used for **basic initialization**
* ❌ Don’t access DOM here

---

**2. connectedCallback()**

* Called when component is **inserted into the DOM**
* Used for **API calls, fetching data, setting initial values**

---

**3. renderedCallback()**

* Called **after component is rendered**
* Used for **DOM-related work**
* ⚠️ Can run multiple times

---

**4. disconnectedCallback()**

* Called when component is **removed from DOM**
* Used for **cleanup (remove listeners, timers, etc.)**

---

**5. errorCallback()**

* Called when **child component throws error**
* Used for **error handling**

---

#### 🔹 Simple flow

```
constructor → connectedCallback → renderedCallback → (updates → renderedCallback again) → disconnectedCallback
```

---

#### 🎯 One-line interview answer

> “LWC lifecycle hooks are methods like constructor, connectedCallback, renderedCallback, and disconnectedCallback that allow us to control component behavior at different stages like initialization, rendering, and cleanup.”

---
---
---
---
---

# Q. Difference between `@wire` and Imperative Apex

#### 🔹 Short, interview-ready answer

**`@wire`** is a **reactive, automatic** way to call Apex and bind data to UI,
while **Imperative Apex** is a **manual call** used when you need full control (like on button click).

---

#### 🔹 Key differences

**@wire**

* Runs **automatically**
* **Reactive** (runs again when params change)
* Good for **displaying data in UI**
* Supports **caching**

**Imperative Apex**

* Called **manually** (on click/event)
* **Not reactive**
* Full control over **when to call and handle response**
* Better for **create/update/delete or conditional calls**

---

#### 🔹 Simple example

**@wire**

```js
@wire(getAccounts) accounts;
```

**Imperative**

```js
getAccounts()
  .then(result => { this.data = result; })
  .catch(error => { this.error = error; });
```

---

#### 🎯 One-line interview answer

> “@wire is used for automatic, reactive data fetching for UI, while imperative Apex is used for manual, controlled calls like button actions or data updates.”

---
---
---
---
---

# Q. How do you optimize LWC performance?

#### 🔹 Short, interview-ready answer

To optimize LWC performance, I focus on **reducing server calls, minimizing re-renders, and handling large data efficiently**.

---

#### 🔹 Key techniques

* **Use `@wire` with caching / LDS**
  → reduces unnecessary server calls

* **Lazy loading components**
  → load only when needed

* **Pagination or infinite scroll**
  → don’t load large datasets at once

* **Avoid unnecessary re-rendering**
  → use only required reactive properties

* **Use `key` in `for:each` loops**
  → helps efficient DOM updates

* **Debounce user input (like search)**
  → avoid multiple server calls on each keystroke

* **Use @track carefully**

* **Use cacheable=true in Apex**

* **Minimize DOM size & nested components**

---

#### 🎯 One-line interview answer

> “I optimize LWC performance by reducing server calls using caching and LDS, avoiding unnecessary re-renders, and handling large data using pagination or lazy loading.”

---
---
---
---
---

# Q. Difference between `@wire` and Imperative Apex

#### 🔹 Short, interview-ready answer

**`@wire`** is **automatic and reactive**, mainly for **reading/displaying data**.
**Imperative Apex** is **manual**, used when you need **control (like on button click or DML)**.

---

#### 🔹 Key differences

**@wire**

* Runs **automatically**
* **Reactive** (re-runs when params change)
* Good for **GET/read data**
* Supports **cache**

**Imperative Apex**

* Called **manually** (on click/event)
* **Not reactive**
* Full control over **when/how to call**
* Used for **create/update/delete or conditional calls**

---

#### 🔹 Simple example

**@wire**

```js
@wire(getAccounts, { industry: '$industry' }) accounts;
```

**Imperative**

```js
handleClick() {
  getAccounts({ industry: this.industry })
    .then(result => { this.data = result; })
    .catch(error => { this.error = error; });
}
```

---

#### 🎯 One-line answer

> “@wire is for automatic, reactive data fetching for UI, while imperative Apex is for manual, controlled calls like button actions or DML operations.”

---
---
---
---
---

# Q. How do you prevent recursive trigger execution?

#### 🔹 Short, interview-ready answer

To prevent recursion in triggers, I **control re-execution using static variables or record tracking and run logic only when needed**.

---

#### 🔹 Common approaches

**1. Static Set of Record Ids (best practice)**

* Track which records are already processed in current transaction

```apex
public class TriggerHelper {
    public static Set<Id> processedIds = new Set<Id>();
}
```

```apex
for(Account acc : Trigger.new){
    if(!TriggerHelper.processedIds.contains(acc.Id)){
        TriggerHelper.processedIds.add(acc.Id);
        // logic
    }
}
```

---

**2. Static Boolean flag (simple cases)**

* Run logic only once per transaction
  ⚠️ Not ideal for bulk records

```apex
public class TriggerHelper {
    public static Boolean isFirstRun = true;
}
```

---

**3. Field change check**

* Run logic only when a specific field actually changes

```apex
if(oldMap.get(acc.Id).Status__c != acc.Status__c){
   // logic
}
```

---

**4. Use trigger framework**

* Centralized control helps manage recursion cleanly

---

#### 🎯 One-line interview answer

> “I prevent recursive trigger execution by using static variables or a Set of processed record IDs and by checking field changes, so the logic runs only once per record per transaction.”

---
---
---
---
---

# Q. Difference between `with sharing`, `without sharing`, and `inherited sharing`

#### 🔹 Short, interview-ready answer

These keywords control **record-level access (sharing rules)** when Apex runs.

---

#### 🔹 with sharing

* Enforces **sharing rules, roles, OWD**
* User can access only the records they are allowed to see

👉 Used for **UI/business logic in user context**

---

#### 🔹 without sharing

* Ignores sharing rules (runs in **system mode**)
* Can access all records

👉 Used for **admin tasks, data processing, batch jobs**

---

#### 🔹 inherited sharing (best modern practice)

* Follows the **sharing of the calling class**
* Acts like **with sharing or without sharing** depending on context

👉 Good for **service classes used by both UI and batch**

---

#### 🔹 Important note

* These control only **record-level access**
* They do **NOT enforce FLS or object permissions** (handle separately)

---

#### 🎯 One-line interview answer

> “With sharing enforces user access, without sharing ignores sharing rules, and inherited sharing follows the context of the calling class.”

---
---
---
---
---

# Q. Explain the complete Order of Execution in Salesforce

#### 🔹 Short, interview-ready answer

Order of Execution is the **sequence in which Salesforce processes a record when it is created or updated**, including validation, triggers, workflows, flows, and database commit.

---

#### 🔹 Main flow (step by step)

1. **System Validation**

   * Required fields, data types, field length

2. **Before Triggers**

   * `before insert`, `before update`

3. **Custom Validation Rules**

   * User-defined validations

4. **Duplicate Rules**

   * Check for duplicate records

5. **Save record (not committed yet)**

6. **After Triggers**

   * `after insert`, `after update`

7. **Assignment Rules**

   * For Leads/Cases

8. **Auto-response Rules**

   * Emails sent

9. **Workflow Rules**

   * Field updates, tasks, emails
     ⚠️ If field update happens → triggers may run again

10. **Process Builder / Flows**

* Additional automation logic

11. **Escalation Rules**

12. **Roll-up Summary Calculations**

13. **Sharing Rules applied**

14. **Commit to Database**

* Record permanently saved

15. **Post-commit logic**

* Emails sent, **@future**, Queueable, Batch, Platform Events

---

#### 🎯 One-line interview answer

> “Order of Execution is the sequence Salesforce follows when saving a record, starting from validation and triggers, then workflows and flows, and finally committing the record and running async operations.”


---
---
---
---
---
# Q: Difference between Custom Metadata vs Custom Settings

#### 🔹 Short, interview-ready answer

Both are used to store **configuration data**, but **Custom Metadata is preferred in modern Salesforce** because it is **deployable and more flexible**.

---

#### 🔹 Key differences

**Custom Metadata Types**

* **Deployable** (can move with change set / package)
* **Cached** → no SOQL needed (`getAll()`)
* Can be used in **formulas, validation rules, flows**
* Best for **business configuration**

---

**Custom Settings**

* **Not deployable with data** (data must be recreated in prod)
* Needs **SOQL (for list)** or `getOrgDefaults()` (for hierarchy)
* Two types: **List** and **Hierarchy**
* Used for **older implementations or user-level settings**

---

#### 🔹 Simple example

* Custom Metadata → mapping like *Customer Type → Discount %*
* Custom Setting → storing *user-specific config (e.g., default region)*

---

#### 🎯 One-line interview answer

> “Custom Metadata is deployable and better for business configuration, while Custom Settings is older and mainly used for runtime or user-specific settings.”
