# 🔷 SET COLLECTION IN APEX

## 1️⃣ What is a Set in Apex?

A **Set** is an **unordered collection** that:

* Stores **UNIQUE values only**
* Does **NOT maintain index**
* Is optimized for **fast lookup**

### Syntax

```apex
Set<String> citySet = new Set<String>();
Set<Id> accountIds = new Set<Id>();
Set<Account> accSet = new Set<Account>();
```

---

## 2️⃣ Why Sets are CRITICAL in Salesforce?

Because Salesforce:

* Works on **bulk records**
* Requires **duplicate removal**
* Needs **IN clause** for SOQL
* Enforces **CPU + query limits**

👉 **Every optimized Apex class uses Set somewhere.**

---

## 3️⃣ How Set Behaves (VERY IMPORTANT)

### 🔹 No Duplicates Allowed

```apex
Set<String> s = new Set<String>();
s.add('Delhi');
s.add('Delhi');

System.debug(s.size()); // 1
```

---

### 🔹 No Order Guarantee

```apex
System.debug(s); // Random order
```

❌ Never rely on order in a Set.

---

## 4️⃣ Creating Sets (All Ways)

### 1. Empty Set

```apex
Set<Integer> nums = new Set<Integer>();
```

### 2. With Values

```apex
Set<Integer> nums = new Set<Integer>{1,2,3};
```

### 3. From List (Duplicate Removal)

```apex
List<String> listData = new List<String>{'A','B','A'};
Set<String> uniqueData = new Set<String>(listData);
```

---

### 🔥 Interview Tip

> Converting List → Set is the **fastest way to remove duplicates**.

---

## 5️⃣ Common Set Methods

| Method       | Purpose        |
| ------------ | -------------- |
| `add()`      | Add element    |
| `addAll()`   | Add collection |
| `remove()`   | Remove element |
| `contains()` | Check value    |
| `size()`     | Count          |
| `isEmpty()`  | Check empty    |
| `clear()`    | Remove all     |

---

### Example

```apex
Set<Integer> nums = new Set<Integer>{1,2};

nums.add(3);
nums.contains(2); // true
nums.remove(1);
```

---

## 6️⃣ Iterating Over a Set

⚠️ No index allowed

### ✅ Correct Way

```apex
for(String val : citySet) {
    System.debug(val);
}
```

❌ Wrong

```apex
citySet[0]; // ❌ Compile error
```

---

## 7️⃣ Set with SObjects (INTERVIEW GOLD)

### ❗ How uniqueness works?

Salesforce uses **Id** to check uniqueness.

```apex
Set<Account> accSet = new Set<Account>();
accSet.add(new Account(Id='001xx1'));
accSet.add(new Account(Id='001xx1'));

System.debug(accSet.size()); // 1
```

---

### ⚠️ Trap

```apex
accSet.add(new Account(Name='Test'));
accSet.add(new Account(Name='Test'));

System.debug(accSet.size()); // 2 ❗
```

👉 No Id → treated as different records.

---

## 8️⃣ MOST IMPORTANT USE CASE: Set + SOQL

### 🔥 Collect IDs First (BEST PRACTICE)

```apex
Set<Id> accIds = new Set<Id>();

for(Account acc : Trigger.new) {
    accIds.add(acc.Id);
}

List<Contact> cons = [
    SELECT Id, AccountId
    FROM Contact
    WHERE AccountId IN :accIds
];
```

✔ Bulk safe
✔ Fast
✔ Governor-friendly

---

## 9️⃣ Set Operations (VERY IMPORTANT)

### 🔹 Union

```apex
setA.addAll(setB);
```

---

### 🔹 Intersection

```apex
setA.retainAll(setB);
```

---

### 🔹 Difference

```apex
setA.removeAll(setB);
```

---

### 🔥 Example

```apex
Set<Integer> A = new Set<Integer>{1,2,3};
Set<Integer> B = new Set<Integer>{2,3,4};

A.retainAll(B); // {2,3}
```

---

## 🔥 COMPLEX REAL-WORLD TASKS

---

## 🧠 TASK 1: Prevent Duplicate Contacts (Email Based)

### 🔹 Requirement

* Prevent inserting contacts with duplicate Email

### ✅ Solution

```apex
Set<String> emails = new Set<String>();

for(Contact c : Trigger.new) {
    if(c.Email != null) {
        emails.add(c.Email);
    }
}

List<Contact> existing = [
    SELECT Email FROM Contact WHERE Email IN :emails
];

Set<String> existingEmails = new Set<String>();
for(Contact c : existing) {
    existingEmails.add(c.Email);
}

for(Contact c : Trigger.new) {
    if(existingEmails.contains(c.Email)) {
        c.addError('Duplicate email not allowed');
    }
}
```

---

## 🧠 TASK 2: Find Missing Records Between Two Lists

```apex
Set<Id> setA = new Set<Id>(listA);
Set<Id> setB = new Set<Id>(listB);

setA.removeAll(setB);
// setA now contains missing records
```

---

## 🧠 TASK 3: De-duplicate Large Dataset

```apex
List<Account> accList = [SELECT Name FROM Account];
Set<String> uniqueNames = new Set<String>();

for(Account acc : accList) {
    uniqueNames.add(acc.Name);
}
```

---

## 🧠 TASK 4: Validate Allowed Picklist Values

```apex
Set<String> allowed = new Set<String>{'New','Working','Closed'};

if(!allowed.contains(status)) {
    throw new CustomException('Invalid status');
}
```

---

## 10️⃣ SET vs LIST (INTERVIEW MUST)

| Feature      | List   | Set    |
| ------------ | ------ | ------ |
| Order        | Yes    | No     |
| Duplicates   | Yes    | No     |
| Index        | Yes    | No     |
| Lookup speed | Slower | Faster |
| IN clause    | ❌      | ✅      |

---

## 🔥 INTERVIEW TRAPS (VERY COMMON)

❓ Can Set contain null? → ✅ Yes (only one null)
❓ Can we sort a Set? → ❌ No
❓ Can Set be used in SOQL IN? → ✅ Yes
❓ Can Set have duplicate SObjects? → ❌ If Id same

