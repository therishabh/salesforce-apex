# 🔥 SOSL

👉 **SOSL = Salesforce Object Search Language**

* Ye **search** ke liye hota hai (Google jaisa)
* **Multiple objects** me **ek saath** data dhoondhta hai
* Jab hume **keyword based search** chahiye hota hai tab use karte hain

---

## 🧠 Simple words me

> “Jab mujhe nahi pata data kis object me hai
> bas ek word hai, aur us word se related sab kuch chahiye”
> ➡️ **SOSL**

---

# ❌ SOQL vs ✅ SOSL (Basic Difference)

| Point        | SOQL            | SOSL             |
| ------------ | --------------- | ---------------- |
| Use          | Query           | Search           |
| Objects      | 1 object        | Multiple objects |
| Where clause | Mandatory       | ❌ Not required   |
| Use case     | Exact data      | Keyword search   |
| Example      | Industry = 'IT' | 'Rishabh'        |

---

# 🧪 Basic SOSL Syntax

```apex
FIND 'searchText'
IN ALL FIELDS
RETURNING ObjectName(Field1, Field2)
```

---

## ✅ Basic Example

```apex
List<List<SObject>> results = [
    FIND 'Sales'
    IN ALL FIELDS
    RETURNING Account(Name), Contact(Name)
];
```

📌 Ye kya karega?

* “Sales” word dhundhega
* Account + Contact dono me
* Name fields se result laayega

---

# 🔍 SOSL Result ka Structure (IMPORTANT)

SOSL **single list return nahi karta**
👉 **List of Lists**

```apex
List<List<SObject>> results
```

* `results[0]` → Account records
* `results[1]` → Contact records

---

## 🧠 Result Handle kaise kare?

```apex
List<Account> accList = (List<Account>) results[0];
List<Contact> conList = (List<Contact>) results[1];
```

---

# 🏢 Real Project Example #1

### 🎯 Global Search Box

> User type kare:
> “Rishabh”
>
> Result chahiye:
>
> * Account
> * Contact
> * Lead

---

## ✅ SOSL Query

```apex
String keyword = 'Rishabh';

List<List<SObject>> searchResults = [
    FIND :keyword
    IN ALL FIELDS
    RETURNING
        Account(Id, Name),
        Contact(Id, Name, Email),
        Lead(Id, Name)
];
```

---

# 🔐 SOSL Injection Safe kaise banaye?

## ❌ Galat (Hardcoded)

```apex
FIND 'Rishabh'
```

## ✅ Correct (Bind variable)

```apex
FIND :keyword
```

✔ Injection safe
✔ Best practice

---

# 🔎 Search Scope Options

| Option            | Meaning              |
| ----------------- | -------------------- |
| IN ALL FIELDS     | Har jagah search     |
| IN NAME FIELDS    | Sirf Name            |
| IN EMAIL FIELDS   | Email fields         |
| IN PHONE FIELDS   | Phone                |
| IN SIDEBAR FIELDS | Global search fields |

---

### Example – Name field only

```apex
FIND :keyword
IN NAME FIELDS
RETURNING Account(Name)
```

---

# 🧠 SOSL with WHERE (Advanced)

```apex
FIND :keyword
IN ALL FIELDS
RETURNING Account(Name WHERE Industry = 'IT')
```

📌 Search + filter combo

---

# 🧪 LIMIT use karna

```apex
FIND :keyword
IN ALL FIELDS
RETURNING Contact(Name LIMIT 5)
```

---

# 🔁 SOSL in Apex (Dynamic Example)

```apex
public static List<Account> searchAccounts(String keyword) {

    List<List<SObject>> results = [
        FIND :keyword
        IN NAME FIELDS
        RETURNING Account(Id, Name)
    ];

    return (List<Account>) results[0];
}
```

---

# ⚠️ Important Limitations (Interview Favorite)

❌ SOSL me:

* Aggregate functions nahi
* GROUP BY nahi
* ORDER BY nahi
* Very complex filters nahi

---

# ⚖️ Kab SOQL, kab SOSL?

## ✅ Use SOSL when:

✔ Keyword search
✔ Multiple objects
✔ Global search
✔ Unknown data location

## ❌ Avoid SOSL when:

❌ Exact filtering
❌ Single object
❌ Reporting logic

---

# 🏆 Real-World Use Cases

| Scenario             | Use     |
| -------------------- | ------- |
| Global search bar    | SOSL    |
| CRM universal search | SOSL    |
| Lookup search        | SOSL    |
| Trigger logic        | ❌ Avoid |
| Reports              | SOQL    |

---

# 📌 Interview Ready One-Liners

* “SOSL is used for text-based search across multiple objects.”
* “SOSL returns a list of lists.”
* “SOQL is structured, SOSL is exploratory.”
* “SOSL is faster for global search use cases.”

---

# 🧠 BONUS: SOQL + SOSL Combo (Advanced)

```apex
// SOSL se Id nikalo
// SOQL se full data lao
```

Real projects me heavy logic isi pattern se hota hai 🔥

---
