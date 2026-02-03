## 1️⃣ What is a List in Apex?

A **List** is an **ordered collection** that:

* Can store **duplicate values**
* Maintains **index order (0-based)**
* Can grow or shrink dynamically

### Basic Syntax

```apex
List<String> names = new List<String>();
List<Integer> numbers = new List<Integer>();
List<Account> accList = new List<Account>();
```

---

## 2️⃣ Why Lists are SUPER IMPORTANT in Salesforce?

Because Salesforce:

* Works on **bulk data**
* Enforces **Governor Limits**
* Processes **multiple records at once**

👉 Almost **every trigger**, **batch**, **future**, **queueable**, and **SOQL** returns a **List**.

Example:

```apex
List<Account> accs = [SELECT Id, Name FROM Account];
```

---

## 3️⃣ Different Ways to Create a List

### 1. Empty List

```apex
List<String> cities = new List<String>();
```

### 2. List with Values

```apex
List<String> cities = new List<String>{'Delhi', 'Noida', 'Pune'};
```

### 3. From Another List

```apex
List<String> newCities = new List<String>(cities);
```

### 4. From SOQL

```apex
List<Contact> contacts = [SELECT Id, Email FROM Contact];
```

---

## 4️⃣ Common List Methods (VERY IMPORTANT)

| Method              | Purpose               |
| ------------------- | --------------------- |
| `add()`             | Add single element    |
| `addAll()`          | Add multiple elements |
| `get(index)`        | Get value             |
| `set(index, value)` | Replace value         |
| `remove(index)`     | Remove by index       |
| `size()`            | Count elements        |
| `clear()`           | Remove all            |
| `contains()`        | Check value           |
| `isEmpty()`         | Check empty           |

### Example

```apex
List<Integer> nums = new List<Integer>{10, 20, 30};

nums.add(40);            // [10,20,30,40]
nums.remove(1);          // removes 20
Integer x = nums.get(0); // 10
```

---

## 5️⃣ Iterating Over a List (BEST PRACTICES)

### 1. For-each Loop (Recommended)

```apex
for(Account acc : accList) {
    System.debug(acc.Name);
}
```

### 2. Index-based Loop

```apex
for(Integer i = 0; i < accList.size(); i++) {
    System.debug(accList[i].Name);
}
```

---

## 6️⃣ List with SObjects (REAL SALESFORCE USAGE)

### Insert Multiple Records (Bulk Safe)

```apex
List<Account> accToInsert = new List<Account>();

for(Integer i = 1; i <= 5; i++) {
    accToInsert.add(new Account(Name = 'Test Acc ' + i));
}

insert accToInsert;
```

✔ Bulk-safe
✔ Governor-limit friendly

---

## 7️⃣ List + SOQL + DML (CORE INTERVIEW PATTERN)

### ❌ BAD PRACTICE (SOQL inside loop)

```apex
for(Account acc : accList) {
    List<Contact> cons = [SELECT Id FROM Contact WHERE AccountId = :acc.Id];
}
```

### ✅ GOOD PRACTICE

```apex
Set<Id> accIds = new Set<Id>();
for(Account acc : accList) {
    accIds.add(acc.Id);
}

List<Contact> cons = [
    SELECT Id, AccountId
    FROM Contact
    WHERE AccountId IN :accIds
];
```

---

## 8️⃣ Advanced List Operations

### 1. Remove Duplicates from List

```apex
List<String> names = new List<String>{'A','B','A','C'};
names = new List<String>(new Set<String>(names));
```

---

### 2. Sorting a List

```apex
List<Integer> nums = new List<Integer>{5,1,3};
nums.sort(); // [1,3,5]
```

---

### 3. Reverse List

```apex
nums.reverse();
```

---

## 9️⃣ VERY IMPORTANT: List vs Set vs Map

| Feature     | List | Set | Map |
| ----------- | ---- | --- | --- |
| Order       | Yes  | No  | No  |
| Duplicates  | Yes  | No  | No  |
| Key-Value   | No   | No  | Yes |
| Index-based | Yes  | No  | No  |

👉 Rule of Thumb:

* **Order needed → List**
* **Uniqueness → Set**
* **Fast lookup → Map**

---

# 🔥 COMPLEX REAL-WORLD TASKS (WITH SOLUTIONS)

---

## 🧠 TASK 1: Update Accounts based on Contacts

### 🔹 Requirement:

* Find all Accounts that have **more than 3 Contacts**
* Update Account field `Has_Many_Contacts__c = true`

### ✅ Solution

```apex
List<Account> accounts = [SELECT Id FROM Account];
Set<Id> accIds = new Set<Id>();

for(Account acc : accounts) {
    accIds.add(acc.Id);
}

List<Contact> contacts = [
    SELECT Id, AccountId
    FROM Contact
    WHERE AccountId IN :accIds
];

Map<Id, Integer> countMap = new Map<Id, Integer>();

for(Contact c : contacts) {
    if(!countMap.containsKey(c.AccountId)) {
        countMap.put(c.AccountId, 1);
    } else {
        countMap.put(c.AccountId, countMap.get(c.AccountId) + 1);
    }
}

List<Account> accToUpdate = new List<Account>();
for(Id accId : countMap.keySet()) {
    if(countMap.get(accId) > 3) {
        accToUpdate.add(
            new Account(Id = accId, Has_Many_Contacts__c = true)
        );
    }
}

update accToUpdate;
```

---

## 🧠 TASK 2: Split Large List into Chunks (Batch Logic)

### 🔹 Requirement:

Process records in **chunks of 200**

### ✅ Solution

```apex
Integer chunkSize = 200;
List<Account> bigList = [SELECT Id FROM Account];

for(Integer i = 0; i < bigList.size(); i += chunkSize) {
    Integer end = Math.min(i + chunkSize, bigList.size());
    List<Account> chunk = bigList.subList(i, end);
    // process chunk
}
```

---

## 🧠 TASK 3: Compare Two Lists and Find Missing Records

### 🔹 Requirement:

Find Accounts present in List A but not in List B

### ✅ Solution

```apex
List<Id> listA = new List<Id>{'001xx1','001xx2','001xx3'};
List<Id> listB = new List<Id>{'001xx2'};

Set<Id> setB = new Set<Id>(listB);
List<Id> missing = new List<Id>();

for(Id idVal : listA) {
    if(!setB.contains(idVal)) {
        missing.add(idVal);
    }
}
```

---

## 🧠 TASK 4: Flatten Child Records into Parent List

### 🔹 Requirement:

Get all Contacts from list of Accounts

```apex
List<Contact> allContacts = new List<Contact>();

for(Account acc : accList) {
    allContacts.addAll(acc.Contacts);
}
```

---

## 1️⃣0️⃣ INTERVIEW GOLD QUESTIONS (LIST)


## ❓1) Difference between `add()` and `addAll()`

### 🔹 `add()`

Adds **ONE element** to the list.

```apex
List<String> names = new List<String>();
names.add('Ravi');
```

✔ Adds single item
❌ Cannot add another list

---

### 🔹 `addAll()`

Adds **MULTIPLE elements** at once (another list or set).

```apex
List<String> list1 = new List<String>{'A','B'};
List<String> list2 = new List<String>{'C','D'};

list1.addAll(list2);
// Result → ['A','B','C','D']
```

✔ Bulk insertion
✔ Cleaner & faster
✔ Preferred in real projects

---

### 🔥 Interview Trick

```apex
list.add(list2);     // ❌ Compile error
list.addAll(list2); // ✅ Correct
```

---

### 🧠 Best Practice

* Single value → `add()`
* Collection → `addAll()`

---

## ❓2) What happens if index is out of range?

Apex **throws a runtime exception**.

### ❌ Example

```apex
List<Integer> nums = new List<Integer>{10,20};

Integer x = nums[5]; // ❌ Exception
```

### 🔴 Error

```
System.ListException: List index out of bounds
```

---

### ❌ Remove invalid index

```apex
nums.remove(3); // ❌ Exception
```

---

### ✅ Safe Way

```apex
if(index < nums.size()) {
    nums.remove(index);
}
```

---

### 🔥 Interview Insight

* Apex **does NOT auto-handle**
* Always check `.size()`

---

## ❓3) `subList()` vs `clone()`

This one is **VERY IMPORTANT** 🔥

---

### 🔹 `subList(start, end)`

Returns a **PARTIAL view** of the original list.

```apex
List<Integer> nums = new List<Integer>{1,2,3,4,5};
List<Integer> sub = nums.subList(1,4); // [2,3,4]
```

⚠️ **Linked to original list**

```apex
sub[0] = 99;
System.debug(nums); // [1,99,3,4,5]
```

---

### 🔹 `clone()`

Creates a **SHALLOW COPY** of the list.

```apex
List<Integer> copy = nums.clone();
```

✔ Independent list
✔ Safe to modify

---

### 🔥 Key Difference Table

| Feature            | subList          | clone            |
| ------------------ | ---------------- | ---------------- |
| Copy Type          | View             | Copy             |
| Linked to original | ✅ Yes            | ❌ No             |
| Use case           | Chunk processing | Safe duplication |

---

### 🧠 Best Practice

* Chunk processing → `subList()`
* Independent list → `clone()`

---

## ❓4) List inside Trigger Context

### 🔹 Trigger Lists

Salesforce automatically provides:

| Context Variable | Type              |
| ---------------- | ----------------- |
| `Trigger.new`    | `List<SObject>`   |
| `Trigger.old`    | `List<SObject>`   |
| `Trigger.newMap` | `Map<Id,SObject>` |
| `Trigger.oldMap` | `Map<Id,SObject>` |

---

### ❌ BAD PRACTICE

```apex
for(Account acc : Trigger.new) {
    insert new Contact(LastName='Test', AccountId=acc.Id); // ❌ DML inside loop
}
```

---

### ✅ GOOD PRACTICE

```apex
List<Contact> cons = new List<Contact>();

for(Account acc : Trigger.new) {
    cons.add(new Contact(LastName='Test', AccountId=acc.Id));
}

insert cons;
```

✔ Bulk safe
✔ Governor-limit safe

---

### 🔥 Interview Trap

* `Trigger.new` is **read-only** in AFTER triggers
* You **can modify** records in BEFORE triggers

---

## ❓5) List of Lists (Nested Collections)

### 🔹 Syntax

```apex
List<List<Integer>> matrix = new List<List<Integer>>();
```

---

### 🔹 Example

```apex
List<Integer> row1 = new List<Integer>{1,2};
List<Integer> row2 = new List<Integer>{3,4};

List<List<Integer>> matrix = new List<List<Integer>>();
matrix.add(row1);
matrix.add(row2);
```

---

### 🔹 Accessing Values

```apex
Integer val = matrix[1][0]; // 3
```

---

### 🔥 Real Salesforce Use Case

✔ Chunking records
✔ Grouping records by batch
✔ Multi-level processing

---

### 🔹 Chunk Example

```apex
List<List<Account>> batches = new List<List<Account>>();

Integer size = 200;
for(Integer i = 0; i < accList.size(); i += size) {
    batches.add(accList.subList(i, Math.min(i+size, accList.size())));
}
```

---

### ⚠️ Warning

* Avoid deep nesting (List<List<List<...>>>)
* Increases complexity & memory usage

---

## 🔥 QUICK INTERVIEW SUMMARY (MEMORIZE THIS)

✔ `add()` → single element
✔ `addAll()` → collection
✔ Out of range → runtime exception
✔ `subList()` → linked view
✔ `clone()` → safe copy
✔ Triggers → always bulk process
✔ Nested Lists → chunking & grouping

---
