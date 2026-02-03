🔥 YES — this is the **MOST IMPORTANT collection in Apex**, Rishabh
If you truly master **Map**, you’ll cross from *developer* to *architect-level thinking* in Salesforce.

I’ll explain **Map Collection** in the **same deep, practical, interview-ready way**:

* Basics → behavior → methods
* Real Salesforce patterns
* Trigger + SOQL usage
* Complex real-world tasks
* Interview traps

Take this slowly — **Map is power** 💪

---

# 🔶 MAP COLLECTION IN APEX (ZERO → ARCHITECT)

---

## 1️⃣ What is a Map in Apex?

A **Map** stores data in **key → value** pairs.

```text
Key  → Value
```

### Syntax

```apex
Map<Id, Account> accMap = new Map<Id, Account>();
Map<String, Integer> countMap = new Map<String, Integer>();
```

---

## 2️⃣ Why Map is KING in Salesforce 👑

Because Salesforce:

* Needs **fast record lookup**
* Processes **bulk triggers**
* Requires **grouping & aggregation**
* Must avoid **SOQL/DML inside loops**

👉 Almost **every optimized trigger** uses Map.

---

## 3️⃣ Core Properties of Map (MUST KNOW)

| Property   | Behavior         |
| ---------- | ---------------- |
| Keys       | UNIQUE           |
| Values     | Can duplicate    |
| Order      | NOT guaranteed   |
| Lookup     | O(1) (very fast) |
| Null key   | ❌ Not allowed    |
| Null value | ✅ Allowed        |

---

### 🔥 Interview Trap

```apex
map.put(null, acc); // ❌ Exception
map.put('A', null); // ✅ Allowed
```

---

## 4️⃣ Creating Maps (All Ways)

### 1️⃣ Empty Map

```apex
Map<Id, Account> accMap = new Map<Id, Account>();
```

---

### 2️⃣ Map with Values

```apex
Map<String, Integer> scoreMap = new Map<String, Integer>{
    'Ravi' => 80,
    'Amit' => 90
};
```

---

### 3️⃣ Map from SOQL (🔥 MOST USED)

```apex
Map<Id, Account> accMap =
    new Map<Id, Account>([SELECT Id, Name FROM Account]);
```

✔ Auto-maps `Id → SObject`

---

## 5️⃣ Common Map Methods

| Method          | Purpose      |
| --------------- | ------------ |
| `put()`         | Add / update |
| `get()`         | Fetch value  |
| `containsKey()` | Check key    |
| `keySet()`      | All keys     |
| `values()`      | All values   |
| `remove()`      | Remove key   |
| `size()`        | Count        |
| `clear()`       | Remove all   |

---

### Example

```apex
map.put('A', 10);
Integer x = map.get('A');
```

---

## 6️⃣ Iterating Over a Map (INTERVIEW GOLD)

### ✅ Best Practice (keySet)

```apex
for(Id accId : accMap.keySet()) {
    Account acc = accMap.get(accId);
}
```

---

### ❌ Bad Practice

```apex
for(Account acc : accMap.values()) {
    // no key access
}
```

---

## 7️⃣ Map in Trigger Context (CRITICAL)

### Salesforce Gives You Maps Automatically

| Variable         | Type               |
| ---------------- | ------------------ |
| `Trigger.newMap` | `Map<Id, SObject>` |
| `Trigger.oldMap` | `Map<Id, SObject>` |

---

### 🔥 Example

```apex
for(Id accId : Trigger.newMap.keySet()) {
    Account newAcc = Trigger.newMap.get(accId);
    Account oldAcc = Trigger.oldMap.get(accId);
}
```

✔ Compare old vs new
✔ No SOQL required

---

## 8️⃣ MOST COMMON PATTERN: Grouping using Map

### 🔹 Count Contacts per Account

```apex
Map<Id, Integer> countMap = new Map<Id, Integer>();

for(Contact c : contacts) {
    countMap.put(
        c.AccountId,
        (countMap.containsKey(c.AccountId) ? countMap.get(c.AccountId) : 0) + 1
    );
}
```

---

## 9️⃣ Map + List + Set (REAL POWER COMBO)

### 🔹 Accounts with More Than 3 Contacts

```apex
Set<Id> accIds = new Set<Id>();
for(Account acc : Trigger.new) accIds.add(acc.Id);

List<Contact> cons = [
    SELECT AccountId FROM Contact WHERE AccountId IN :accIds
];

Map<Id, Integer> countMap = new Map<Id, Integer>();

for(Contact c : cons) {
    countMap.put(c.AccountId,
        countMap.containsKey(c.AccountId) ? countMap.get(c.AccountId)+1 : 1
    );
}

List<Account> updateList = new List<Account>();
for(Id accId : countMap.keySet()) {
    if(countMap.get(accId) > 3) {
        updateList.add(new Account(Id=accId, VIP__c=true));
    }
}
update updateList;
```

---

## 🔥 COMPLEX REAL-WORLD TASKS

---

## 🧠 TASK 1: Detect Field Change in Trigger

```apex
for(Id accId : Trigger.newMap.keySet()) {
    Account newAcc = Trigger.newMap.get(accId);
    Account oldAcc = Trigger.oldMap.get(accId);

    if(newAcc.Status__c != oldAcc.Status__c) {
        // status changed
    }
}
```

---

## 🧠 TASK 2: Parent → Children Grouping

```apex
Map<Id, List<Contact>> accToContacts = new Map<Id, List<Contact>>();

for(Contact c : contacts) {
    if(!accToContacts.containsKey(c.AccountId)) {
        accToContacts.put(c.AccountId, new List<Contact>());
    }
    accToContacts.get(c.AccountId).add(c);
}
```

---

## 🧠 TASK 3: Prevent Duplicate Records (Composite Key)

```apex
Map<String, Contact> uniqueMap = new Map<String, Contact>();

for(Contact c : Trigger.new) {
    String key = c.Email + '-' + c.AccountId;

    if(uniqueMap.containsKey(key)) {
        c.addError('Duplicate Contact');
    } else {
        uniqueMap.put(key, c);
    }
}
```

---

## 10️⃣ Map vs List vs Set (INTERVIEW MUST)

| Feature      | List    | Set    | Map        |
| ------------ | ------- | ------ | ---------- |
| Order        | Yes     | No     | No         |
| Duplicates   | Yes     | No     | Keys ❌     |
| Index        | Yes     | No     | No         |
| Lookup Speed | ❌       | ✅      | 🔥 Fastest |
| Trigger use  | Limited | Medium | 🔥 Max     |

---

## 🔥 INTERVIEW TRAPS (VERY COMMON)

- ❓ Can Map key be null? → ❌
- ❓ Can Map value be null? → ✅
- ❓ Does Map maintain order? → ❌
- ❓ Best way to avoid SOQL in loop? → Map
- ❓ Trigger.newMap vs new Map<Id,SObject>? → Provided by Salesforce
