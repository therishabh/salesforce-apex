# Introduction Queueable Apex

* Queueable apex is also part of **Async apex** and we can create any apex class as a queueable class by implementing the **System.Queueable interface**.
* Must need to implement `execute(QueueableContext context)` – **Heart of Queueable Method**.
* The Queueable interface lets us add the jobs in the queue and lets them monitor.
* `System.enqueueJob()` method is used to add a job in Queue and returns an **Id to Monitor the Job**.
* Queueable Job can accept both **primitive and non-primitive data types** as input.
* We can chain **multiple Queueable jobs** by invoking another Queueable Job from the running Queueable Job and this is helpful when the outcome for 1 job is the input of another job.
* We can call **future methods from Salesforce Queueable Apex**.

---
## Queueable Apex Limits

* You can add up to **50 jobs to the queue** with `System.enqueueJob` in a **single transaction`.
* There is **no depth limit for chaining the job**, which means we can chain **as many jobs as we want**.
* For the **Developer Edition**, the **max depth for the chaining job is 5**.
* When calling/chaining jobs with `System.enqueueJob` method, **you can add only one job from a parent job**.


## Queueable vs Future

| **Future**                                            | **Queueable**                                              |
| ----------------------------------------------------- | ---------------------------------------------------------- |
| Future is a **Method**                                | Queueable is **Apex Class**                                |
| Can not accept the **non-primitive data types**       | Can accept both **primitive and non-primitive data types** |
| Can not call the future method from the future method | The future method can be called from **Queueable apex**    |
| We can not track the future method                    | We can **easily monitor the jobs**                         |
| Method chaining is not possible                       | **Method / Job chaining is possible**                      |
| Possibility of Data loss                              | Possibility of Data loss                                   |


---
---

# 🔥 Queueable Apex kya hota hai?

**Queueable Apex** ek **advanced asynchronous mechanism** hai jo:

* Background me run hota hai
* `@future` se **zyada powerful** hai
* Complex logic + chaining allow karta hai

Simple words me:

> “Future Apex ka upgraded, enterprise-ready version.”

---

# 🔁 Sync vs Future vs Queueable (Mental Model)

| Feature      | Synchronous | @future        | Queueable                |
| ------------ | ----------- | -------------- | ------------------------ |
| Thread       | Same        | Separate       | Separate                 |
| Params       | Any         | Primitive only | **Any (sObject, class)** |
| Chaining     | ❌           | ❌              | ✅                        |
| Job tracking | ❌           | ❌              | ✅                        |
| Control      | Low         | Low            | **High**                 |
| Industry use | Core        | Legacy         | **Preferred**            |

---

## Queueable Apex ka basic structure

### Step 1: Class implements `Queueable`

```apex
public class AccountQueueable implements Queueable {

    public void execute(QueueableContext context) {
        System.debug('Queueable running');
        System.debug('Job Id' + context.getJobId());
    }
}
```

---

### Step 2: Job enqueue karna

```apex
System.enqueueJob(new AccountQueueable());
```

📌 Ye line job ko **Salesforce async queue** me daal deti hai.

---

## 🔥 Queueable ka sabse bada advantage – PARAMETERS

### ❌ @future limitation

* sObject pass nahi kar sakte

### ✅ Queueable me allowed

* sObject
* List<Account>
* Map<Id, Account>
* Custom Apex class

#### Example

```apex
public class AccountQueueable implements Queueable {

    private List<Account> accounts;

    public AccountQueueable(List<Account> accs) {
        this.accounts = accs;
    }

    public void execute(QueueableContext qc) {
        for (Account acc : accounts) {
            acc.Description = 'Processed Async';
        }
        update accounts;
    }
}
```

---

## 🔥 Real Project Example (Trigger → Queueable)

### 🎯 Requirement:

> Account create hone ke baad
> heavy calculation + related records update
> user ko wait na karna pade

---

## Trigger (Industry style)

```apex
trigger AccountTrigger on Account (after insert) {

    if (Trigger.isAfter && Trigger.isInsert) {
        System.enqueueJob(
            new AccountQueueable(Trigger.new)
        );
    }
}
```

* 📌 Trigger clean
* 📌 Heavy logic async

---

## 🔥 CHAINING – Queueable ka killer feature ⭐

Queueable **dusre Queueable ko call** kar sakta hai. But isme ek limitation hai ki kewal ek hi Queueable ko hi call kr skta hai multiple Queueable ko call nahi kr skta.

```apex
public void execute(QueueableContext qc) {

    // First job logic

    System.enqueueJob(new SecondQueueable());
}
```

📌 Use cases:

* Step-wise processing
* Retry mechanism
* Long workflows

---

## 🔥 Queueable with Callouts (Very Common)

External API call ke liye:

```apex
public class CalloutQueueable
    implements Queueable, Database.AllowsCallouts {

    public void execute(QueueableContext qc) {

        HttpRequest req = new HttpRequest();
        req.setEndpoint('https://api.example.com');
        req.setMethod('GET');

        Http http = new Http();
        HttpResponse res = http.send(req);
    }
}
```

📌 `Database.AllowsCallouts` mandatory

---

## 🔥 Job Monitoring & Tracking (BIG DIFFERENCE vs Future)

Queueable job ka **Job Id milta hai**:

```apex
Id jobId = System.enqueueJob(new AccountQueueable());
```

Is `jobId` se:

* `AsyncApexJob` query
* Job status track
* Error analysis

```apex
SELECT Id, Status, JobType
FROM AsyncApexJob
WHERE Id = :jobId
```

👉 Ye **Future me possible nahi** ❌

---

## 🔥 Governor Limits – Queueable

| Limit                          | Value         |
| ------------------------------ | ------------- |
| Queueable jobs per transaction | **50**        |
| SOQL/DML                       | Async limits  |
| Heap size                      | Higher        |
| Chained jobs                   | 1 per execute |

⚠️ Loop ke andar `enqueueJob` ❌ (limit hit)

---

## 🔥 Error Handling (Production Ready)

```apex
public void execute(QueueableContext qc) {
    try {
        // logic
    } catch (Exception e) {
        Error_Log__c log = new Error_Log__c(
            Message__c = e.getMessage()
        );
        insert log;
    }
}
```

- 📌 Async error user ko nahi dikhta
- 📌 Logging mandatory

---

## 🔥 Testing Queueable Apex (Interview Favorite)

## Test class ka golden rule

```apex
@isTest
public class AccountQueueableTest {

    @isTest
    static void testQueueable() {

        Account acc = new Account(Name='Test');
        insert acc;

        Test.startTest();
        System.enqueueJob(
            new AccountQueueable(
                new List<Account>{acc}
            )
        );
        Test.stopTest(); // executes job
    }
}
```

📌 `Test.stopTest()` → job force execute

---

## 🔥 Queueable vs Batch Apex

| Queueable         | Batch            |
| ----------------- | ---------------- |
| Small–medium data | Huge data (50k+) |
| Immediate async   | Scheduled / long |
| Chaining          | Start → finish   |
| Easy to debug     | More complex     |

👉 Rule of thumb:

* **< 10k records → Queueable**
* **> 50k records → Batch**

---

# 🔥 13. Disadvantages of Queueable ❌

- 🚫 Slightly more complex than future
- 🚫 Still async → no immediate result
- 🚫 Overuse can flood async queue
- 🚫 Not for massive data volumes

---

# 🔥 14. Industry Best Practices (MUST FOLLOW)

- ✔ Prefer Queueable over Future
- ✔ Trigger me sirf enqueue
- ✔ One job = one responsibility
- ✔ Use chaining carefully
- ✔ Proper error logging
- ✔ Monitor AsyncApexJob
- ✔ Avoid enqueue in loops

---

# 🔥 15. Interview-Ready One-Liners

* “Queueable Apex is a more flexible replacement for Future.”
* “Queueable supports complex parameters and chaining.”
* “Queueable jobs can be monitored via AsyncApexJob.”
* “Queueable is ideal for trigger-based async processing.”

---

# 🧠 Final Verdict (Real Project Advice)

- ✔ **New development → Queueable**
- ✔ **Future → migrate to Queueable**
- ✔ **Batch only for very large datasets**
- ✔ **Clean trigger + async = scalable org**

---
---
---
---
---
---


# 🎤 INTERVIEW-READY ANSWER (Queueable Apex – 5+ Years)

### ❓ *“When do you use Queueable Apex in your project?”*

> **Answer (Natural, real-world tone):**

“In our projects, I use Queueable Apex when I need more control over asynchronous processing than what `@future` provides. Typically, when the logic is slightly complex, needs chaining, or involves passing custom objects or multiple parameters, Queueable becomes the preferred choice.

For example, we had a scenario where after a record update, we had to perform a callout, process the response, and then update multiple related records based on that response. Since this required multiple steps and better control over execution flow, we implemented it using Queueable Apex.”

---

## 🔍 REAL-WORLD SCENARIOS (Mention 1–2)

### ✅ Scenario 1: Multi-Step Async Processing

> “In one integration, after sending data to an external system, we needed to process the response and then update different Salesforce records accordingly. Queueable helped us chain jobs so the next step executed only after the previous one completed.”

Why this sounds real:

* Response handling
* Sequential execution
* Chaining mentioned

---

### ✅ Scenario 2: Trigger → Async with Parameters

> “Another use case was when we needed to pass a list of record IDs and some context information from a trigger to the async job. Queueable supports passing complex data types, which made the implementation much cleaner.”

---

# ❓ *“Did you face any difficulties while using Queueable Apex?”*

> **Answer (mature, honest):**

“Yes, while Queueable Apex is more powerful than `@future`, it also comes with its own considerations. One challenge we faced was managing queue depth and ensuring we didn’t chain too many jobs, which can hit platform limits. We had to design the logic carefully so that chaining was controlled and predictable.”

---

## 🔥 REAL DIFFICULTIES YOU CAN MENTION

### ⚠️ 1. Queue Limit / Chaining Control

> “If chaining isn’t handled properly, it’s easy to enqueue too many jobs. We had to introduce checks to ensure only one follow-up job was queued when required.”

---

### ⚠️ 2. Error Handling & Debugging

> “Since Queueable runs asynchronously, debugging issues required careful logging and monitoring using debug logs and custom error handling.”

---

### ⚠️ 3. Not Ideal for Very Large Data Sets

> “For large data volumes, Queueable wasn’t the best fit, so we switched to Batch Apex for better scalability.”

---

# 🔄 HOW YOU MADE DECISIONS (VERY IMPORTANT)

> **Key line (must say):**

“We generally follow a decision pattern:

* `@future` for very simple, fire-and-forget tasks
* Queueable Apex when we need chaining or better control
* Batch Apex when dealing with large data volumes.”

🔥 This line shows **architect-level thinking**.

---

# 🧠 SHORT VERSION (IF INTERVIEWER WANTS QUICK ANSWER)

> “I use Queueable Apex when async logic is complex, needs chaining, or requires passing custom data. Compared to `@future`, it gives better control and is easier to manage. One challenge was managing queue limits and chaining carefully, but overall it’s our preferred async option for medium-complexity use cases.”

---

# 🚫 WHAT NOT TO SAY (AVOID THESE)

❌ “Queueable is always better than future”
❌ “We never faced issues”
❌ “We use Queueable for everything”

---

# 🏆 BONUS – If Interviewer Asks:

### ❓ *“Why not always use Queueable instead of @future?”*

> **Answer:**

“For very simple, one-step async operations, `@future` is still simpler and sufficient. Using Queueable everywhere would add unnecessary complexity.”

---
