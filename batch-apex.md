
# 🔥 Batch Apex

**Batch Apex** Salesforce ka mechanism hai jo:

* **Large data volumes** (50k → millions records) ko
* **small chunks (batches)** me
* **asynchronously** process karta hai

Simple words me:

> “Jab data bahut zyada ho aur ek baar me process possible na ho,
> tab Batch Apex use hota hai.”

---

# 🧠 Kyun Batch Apex ki zarurat padi?

Salesforce me limits hoti hain:

* SOQL rows
* DML rows
* CPU time

👉 Agar tum 1 lakh records ko normal Apex se process karoge → **Governor Limit hit** ❌

**Batch Apex solution deta hai:**

* Records ko **200–200 ke scope** me tod deta hai
* Har scope **alag transaction** me run hota hai

---

# 🔁 Sync vs Queueable vs Batch (Mental Model)

| Feature          | Sync      | Queueable | Batch          |
| ---------------- | --------- | --------- | -------------- |
| Data size        | Small     | Medium    | **Very Large** |
| Execution        | Immediate | Async     | Async          |
| Chunking         | ❌         | ❌         | ✅              |
| Millions records | ❌         | ❌         | ✅              |
| Complexity       | Low       | Medium    | High           |

---

# 🔥 Batch Apex ka structure (VERY IMPORTANT)

Batch Apex class **3 mandatory methods** implement karti hai:

```apex
public class MyBatch implements Database.Batchable<SObject> {

    public Database.QueryLocator start(Database.BatchableContext bc) {
        // 1️⃣ Records define
    }

    public void execute(Database.BatchableContext bc, List<SObject> scope) {
        // 2️⃣ Process each batch (default 200)
    }

    public void finish(Database.BatchableContext bc) {
        // 3️⃣ Final work (email / log)
    }
}
```

---

# 🔥 start() method – Data source

## Option 1️⃣: QueryLocator (MOST COMMON)

```apex
public Database.QueryLocator start(Database.BatchableContext bc) {
    return Database.getQueryLocator(
        'SELECT Id, Name FROM Account'
    );
}
```

✔ Supports **millions of records**
✔ Preferred for large data

---

## Option 2️⃣: Iterable (Advanced)

```apex
public Iterable<Account> start(Database.BatchableContext bc) {
    return [SELECT Id FROM Account LIMIT 1000];
}
```

⚠️ Rarely used, QueryLocator better

---

# 🔥 execute() method – Actual processing

```apex
public void execute(Database.BatchableContext bc, List<Account> scope) {

    for (Account acc : scope) {
        acc.Description = 'Processed by Batch';
    }

    update scope;
}
```

📌 `scope` = **batch of records (default 200)**
📌 Har execute() **separate transaction**

---

# 🔥 finish() method – Cleanup & summary

```apex
public void finish(Database.BatchableContext bc) {

    // Send email
    // Update log
    // Start another batch
}
```

📌 finish() **sirf ek baar** run hota hai
📌 Use for:

* Notification
* Chaining
* Audit log

---

# 🔥 Batch size – Control performance

```apex
Database.executeBatch(new MyBatch(), 100);
```

| Batch Size    | Impact          |
| ------------- | --------------- |
| 50            | Safer, slower   |
| 200 (default) | Balanced        |
| 500           | Faster, riskier |

👉 Real projects me **100–200** ideal hota hai.

---

# 🔥 Real Project Example (VERY IMPORTANT)

### 🎯 Requirement:

> Night me 5 lakh Accounts process karne hain
> Rating recalculate karni hai
> User impact nahi hona chahiye

---

```apex
public class AccountRatingBatch
implements Database.Batchable<Account> {

    public Database.QueryLocator start(Database.BatchableContext bc) {
        return Database.getQueryLocator(
            'SELECT Id, AnnualRevenue FROM Account'
        );
    }

    public void execute(Database.BatchableContext bc, List<Account> scope) {

        for (Account acc : scope) {
            if (acc.AnnualRevenue > 1000000) {
                acc.Rating = 'Hot';
            }
        }
        update scope;
    }

    public void finish(Database.BatchableContext bc) {
        // send completion email
    }
}
```

---

# 🔥 Batch with Callouts (Integration)

Callouts ke liye extra interface lagta hai:

```apex
public class CalloutBatch
implements Database.Batchable<Account>,
           Database.AllowsCallouts {

    public void execute(Database.BatchableContext bc, List<Account> scope) {
        // HTTP callout allowed
    }
}
```

📌 Callout har scope ke liye ho sakta hai

---

# 🔥 Stateful Batch (Data preserve karna)

Normally:

* Har execute() independent
* Variables reset ho jaate hain

## Agar data preserve chahiye → `Database.Stateful`

```apex
public class StatefulBatch
implements Database.Batchable<Account>, Database.Stateful {

    Integer totalProcessed = 0;

    public void execute(Database.BatchableContext bc, List<Account> scope) {
        totalProcessed += scope.size();
    }

    public void finish(Database.BatchableContext bc) {
        System.debug('Total: ' + totalProcessed);
    }
}
```

📌 Use case:

* Total count
* Summary data

---

# 🔥 Batch Chaining (Advanced)

finish() se **dusra batch start** kar sakte ho:

```apex
public void finish(Database.BatchableContext bc) {
    Database.executeBatch(new AnotherBatch(), 200);
}
```

📌 Sequential large workflows

---

# 🔥 Governor Limits – Batch Apex

| Limit            | Value                   |
| ---------------- | ----------------------- |
| SOQL rows        | **50 million**          |
| DML rows         | **10k per transaction** |
| execute() runs   | Unlimited               |
| Parallel batches | 5                       |

📌 Har scope → fresh limits

---

# 🔥 Error Handling (Production MUST)

```apex
public void execute(Database.BatchableContext bc, List<Account> scope) {
    try {
        update scope;
    } catch (Exception e) {
        // log error
    }
}
```

✔ Partial failure safe
✔ One scope fail ≠ whole batch fail

---

# 🔥 Testing Batch Apex (Interview Favorite)

```apex
@isTest
public class AccountBatchTest {

    @isTest
    static void testBatch() {

        List<Account> accs = new List<Account>();
        for (Integer i = 0; i < 300; i++) {
            accs.add(new Account(Name='Acc '+i));
        }
        insert accs;

        Test.startTest();
        Database.executeBatch(new AccountRatingBatch(), 100);
        Test.stopTest();

        // Assertions
    }
}
```

📌 `Test.stopTest()` batch ko force execute karta hai

---

# 🔥 Disadvantages of Batch Apex ❌

🚫 Complex structure
🚫 Debugging tough
🚫 Not real-time
🚫 Overkill for small data
🚫 Scheduling required often

---

# 🔥  Batch Apex Best Practices (Industry Rules)

✔ Use only for **large data**
✔ Keep execute() lightweight
✔ Avoid SOQL/DML in loops
✔ Choose correct batch size
✔ Use Stateful only if needed
✔ Log errors
✔ Monitor `AsyncApexJob`

---

# 🔥 Interview-Ready One-Liners

* “Batch Apex processes large data asynchronously in chunks.”
* “Each execute() runs in a separate transaction.”
* “Database.QueryLocator supports millions of records.”
* “Batch Apex is ideal for data cleanup and migrations.”

---

# 🧠 Final Decision Guide

| Scenario           | Best Choice      |
| ------------------ | ---------------- |
| < 10k records      | Queueable        |
| 50k+ records       | **Batch Apex**   |
| Night processing   | Batch + Schedule |
| Trigger logic      | Queueable        |
| One-time migration | Batch            |

---

