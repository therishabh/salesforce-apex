# 🔥 Salesforce me `Database` Methods kyu use karte hain

jab `insert / update / delete` already available hain?

---

## 🧠 Basic samajh lo pehle

Salesforce me **2 tarike** se DML kar sakte ho:

1️⃣ **Simple DML**

```apex
insert accList;
update accList;
delete accList;
```

2️⃣ **Database Methods**

```apex
Database.insert(accList, false);
Database.update(accList, false);
Database.delete(accList, false);
```

---

# ❓ Phir Database Methods ki zarurat kyu padi?

### 👉 **Answer (one line)**

Database methods hume **partial success + error handling + control** dete hain
jo simple DML me possible nahi hai.

---

# 🚨 Simple DML ka sabse bada problem

### ❌ All-or-Nothing behavior

```apex
List<Account> accList = new List<Account>{
    new Account(Name = 'Good Account'),
    new Account(Name = null) // ❌ invalid
};

insert accList;
```

### 💥 Result:

* Ek record invalid hai
* **PURE LIST FAIL**
* **Koi bhi record insert nahi hota**

---

# ✅ Database.insert() ka magic ✨

```apex
Database.SaveResult[] results = Database.insert(accList, false);
```

### `false` = **allOrNone = false**

👉 Matlab:

* Jo valid hai → insert ho jayega
* Jo invalid hai → fail hoga
* System crash nahi karega

---

## 🔍 SaveResult ka use (Industry Standard)

```apex
for (Database.SaveResult sr : results) {
    if (sr.isSuccess()) {
        System.debug('Inserted Id: ' + sr.getId());
    } else {
        for (Database.Error err : sr.getErrors()) {
            System.debug(err.getMessage());
        }
    }
}
```

---

# 🏢 Real Project Scenario (VERY IMPORTANT)

### 🎯 Requirement:

> 200 Accounts insert ho rahe hain trigger se
> Agar 5 fail ho jaayein
> 195 save hone chahiye
> Aur error log hona chahiye

❌ Simple DML → pura batch fail
✅ Database method → partial success + logging

---

# 🔐 Real Use Cases jahan Database Methods MUST hote hain

## 1️⃣ Trigger me (Production org)

```apex
Database.insert(taskList, false);
```

Reason:

* Bulk records
* Ek record fail ho sakta hai
* Business doesn't want rollback

---

## 2️⃣ Integration / API Data

* External system se dirty data aata hai
* Har record perfect nahi hota
* Data loss allowed nahi

---

## 3️⃣ Async Jobs (Queueable / Batch)

* Long running process
* Partial failure acceptable
* Retry logic possible

---

# ⚖️ Simple DML vs Database Methods

| Feature         | Simple DML | Database Methods |
| --------------- | ---------- | ---------------- |
| Syntax          | Simple     | Thoda complex    |
| All-or-none     | ✅ Yes      | ❌ Optional       |
| Partial success | ❌ No       | ✅ Yes            |
| Error handling  | ❌ Limited  | ✅ Full           |
| Return result   | ❌ No       | ✅ SaveResult     |
| Logging         | ❌ Hard     | ✅ Easy           |

---

# ⚠️ Interview Trap Question

❓ *“Trigger me simple DML use kar sakte hain?”*

✔ **Technically YES**
❌ **Production best practice NO**

👉 Industry expects:

```apex
Database.insert(records, false);
```

---

# 🧪 Update & Delete Example

```apex
Database.update(accList, false);
Database.delete(accList, false);
```

Same benefit:

* Partial success
* Error capture

---

# 🧠 Extra Advanced Tip (Senior Level)

### 🔹 `Database.upsert()`

```apex
Database.upsert(accList, Account.External_Id__c, false);
```

Use case:

* External system sync
* Insert + Update together
* Partial failure handling

---

# 📌 Summary (Yaad rakhne wali lines)

✔ Simple DML = **All or nothing**
✔ Database methods = **Control + Safety**
✔ Triggers + Integration = **Always Database methods**
✔ Interviews = **Explain with real scenario**

---