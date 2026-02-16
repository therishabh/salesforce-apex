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

## 🔹 Key ways to ensure code quality

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

# Q: What is a Selective Query in Salesforce?

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

# Q: Why `Test.startTest()` and `Test.stopTest()` are important?

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
# Q: How do you design a scalable trigger framework?

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

# Q: How do you handle 10k records update?

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

# Q: When to use Future vs Queueable vs Batch?

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

# Q: What happens if a Queueable job fails?

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

# Q: Explain LWC lifecycle hooks

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

# Q: Difference between `@wire` and Imperative Apex

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

# Q: How do you optimize LWC performance?

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

# Q: Difference between `@wire` and Imperative Apex

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

# Q: How do you prevent recursive trigger execution?

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

# Q: Difference between `with sharing`, `without sharing`, and `inherited sharing`

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

# Q: Explain the complete Order of Execution in Salesforce

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
