
# @future Asynchronous Apex – Foundation

## @future Apex kya hota hai?

`@future` Apex Salesforce ka **sabse basic asynchronous mechanism** hai.

👉 Matlab:

* Code **turant execute nahi hota**
* Salesforce **background queue** me daal deta hai
* User ko wait nahi karna padta

Simple line me:

> “Heavy ya slow kaam ko user request se alag, background me run karna.”

---

## ❌ Synchronous vs ✅ @future (Quick Contrast)

| Point       | Synchronous | @future    |
| ----------- | ----------- | ---------- |
| Execution   | Turant      | Background |
| User wait   | Yes         | No         |
| Limits      | Tight       | Relaxed    |
| Trigger use | Heavy ❌     | Light ✅    |

---

## @future ka basic syntax

```apex
public class FutureExample {

    @future
    public static void doAsyncWork() {
        System.debug('Running in background');
    }
}
```

📌 **Mandatory rules**:

1. Method **static** honi chahiye
2. Return type **void**
3. `@future` annotation compulsory

---

# 🔥 PART-2: @future ke Rules & Restrictions (VERY IMPORTANT)

Ye section **interview + real project** dono ke liye critical hai.

---

## 1️⃣ Parameters rules (MOST IMPORTANT)

### ❌ Allowed nahi:

* SObject
* List<Account>
* Map<Id, Account>
* Custom class object

### ✅ Allowed:

* Primitive types
* Collection of primitives

```apex
@future
public static void processData(Set<Id> recordIds, String type) {
    // valid
}
```

📌 Reason:
Future method **serialization** ke through background queue me jaata hai.

---

## 2️⃣ Return value allowed nahi

```apex
@future
public static Integer myMethod() { // ❌
    return 1;
}
```

✔ Hamesha `void`

---

## 3️⃣ Future inside Future ❌

```apex
@future
public static void first() {
    second(); // ❌ agar second bhi future hai
}
```

📌 Nested async allowed nahi

---

## 4️⃣ Trigger se future call kar sakte hain?

✔ **Yes**, but **carefully**

❌ Loop ke andar future call = Governor limit hit

```apex
// ❌ WRONG
for(Account acc : Trigger.new){
    FutureClass.method(acc.Id);
}
```

```apex
// ✅ CORRECT
Set<Id> accIds = new Set<Id>();
for(Account acc : Trigger.new){
    accIds.add(acc.Id);
}
FutureClass.method(accIds);
```

---

# 🔥 PART-3: Real Project Use Cases of @future

## 🏢 Use Case 1: Trigger ke baad Callout

### 🎯 Requirement:

> Account create hone ke baad
> External system ko data bhejna
> (Callout trigger me allowed nahi)

---

### Trigger

```apex
trigger AccountTrigger on Account (after insert) {

    Set<Id> accIds = new Set<Id>();
    for(Account acc : Trigger.new){
        accIds.add(acc.Id);
    }

    AccountFuture.sendToExternal(accIds);
}
```

---

### Future Class

```apex
public class AccountFuture {

    @future(callout=true)
    public static void sendToExternal(Set<Id> accIds) {

        List<Account> accList = [
            SELECT Id, Name
            FROM Account
            WHERE Id IN :accIds
        ];

        // HTTP callout here
    }
}
```

📌 `callout=true` mandatory for API calls

---

## 🏢 Use Case 2: Heavy calculation

```apex
@future
public static void recalculateScores(Set<Id> accIds) {
    // CPU heavy logic
}
```

---

# 🔥 PART-4: Limits & Governor Limits for @future

## ⚙️ Important Limits

| Limit                        | Value            |
| ---------------------------- | ---------------- |
| Future calls per transaction | **50**           |
| Callouts allowed             | ✅ Yes            |
| SOQL/DML limits              | Higher than sync |
| Order guarantee              | ❌ No             |

📌 Execution order **guaranteed nahi hota**

---

# 🔥 PART-5: Error Handling in @future

❌ Try-catch se error user ko nahi dikhta
✔ Logs / Custom Object / Email se track karna hota hai

```apex
@future
public static void asyncMethod(Set<Id> ids) {
    try {
        // logic
    } catch (Exception e) {
        System.debug(e.getMessage());
        // Log_Error__c record create
    }
}
```

---


> Note : 
> Future methods are commonly used to resolve Mixed DML errors. Mixed DML occurs when setup objects (User, Profile, PermissionSet, etc.) and non-setup objects (Account, Contact, Custom objects) are modified in the same transaction.

> Future Apex runs in a separate transaction, so it allows setup and non-setup object DML to be separated safely.

# 🔥 PART-6: Testing @future Apex (Interview Favorite)

## ❓ Test class me future kaise test karein?

👉 `Test.startTest()` & `Test.stopTest()` use karo

```apex
@isTest
public class AccountFutureTest {

    @isTest
    static void testFuture() {

        Account acc = new Account(Name = 'Test');
        insert acc;

        Test.startTest();
        AccountFuture.sendToExternal(
            new Set<Id>{ acc.Id }
        );
        Test.stopTest(); // forces future execution

        // assertions
    }
}
```

📌 `Test.stopTest()` ke bina future run hi nahi hota

---

# 🔥 PART-7: Advantages of @future Apex ✅

✔ Simple & easy to write
✔ Background execution
✔ Trigger se heavy kaam hata sakte ho
✔ Callout allowed
✔ Better performance than sync

---

# 🔥 PART-8: Disadvantages of @future Apex ❌

🚫 Limited parameter types
🚫 No chaining
🚫 No return value
🚫 Hard to debug
🚫 Execution order guarantee nahi
🚫 Testing complex
🚫 Legacy approach (Queueable preferred)

---

# 🔥 PART-9: @future vs Queueable (Reality Check)

| Point          | @future        | Queueable  |
| -------------- | -------------- | ---------- |
| Parameters     | Primitive only | Any object |
| Chaining       | ❌              | ✅          |
| Control        | Low            | High       |
| Debugging      | Hard           | Easy       |
| Industry usage | Low            | High       |

👉 **Industry Rule**:

> New development = **Queueable**
> Old code / simple need = **@future**

---

# 🔥 PART-10: Interview Ready One-Liners

* “@future runs Apex asynchronously in a separate transaction.”
* “Future methods must be static and return void.”
* “@future is mainly used for callouts after triggers.”
* “Queueable Apex is a more powerful replacement for @future.”
* Future methods do not return a Job ID because they must return void.
Hence, future jobs cannot be directly monitored using AsyncApexJob,
making debugging and tracking difficult compared to Queueable Apex.
* You cannot call a @future method from Batch Apex execute() method.
However, you can start a Batch Apex job from a @future method.
* Future methods can accept only primitive data types
or collections of primitive types.
They cannot accept sObjects, custom objects, or complex types.


---

# 🧠 Final Recommendation (Real Project Advice)

✔ Existing org me future already use ho → **maintain it**
✔ New feature bana rahe ho → **Queueable use karo**
✔ Simple background callout → future acceptable
✔ Complex logic → avoid future

---

Agar tum chaho, next part me main:

* 🔥 **Queueable Apex deep dive**
* 🔁 **Future → Queueable migration**
* 🧪 **Async error logging framework**
* ❓ **Top async interview questions**

Bas bolo: **“Next part start karo”** 🚀
