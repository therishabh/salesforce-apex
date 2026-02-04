# 🔥 Dynamic SOQL

👉 Jab SOQL query **runtime pe banti hai (string ke form me)**
aur `Database.query()` se execute hoti hai
toh usko **Dynamic SOQL** kehte hain.

---

## ❌ Static SOQL (Normal)

```apex
List<Account> accList = [
    SELECT Id, Name
    FROM Account
    WHERE Industry = 'IT'
];
```

📌 Yahan:

* Query compile time pe fix hai
* Runtime pe change nahi ho sakti

---

## ✅ Dynamic SOQL (Basic Syntax)

```apex
String query = 'SELECT Id, Name FROM Account';
List<SObject> records = Database.query(query);
```

---

# 🧠 Dynamic SOQL kyu use karte hain?

✔ Fields dynamic ho
✔ Object dynamic ho
✔ Filters optional ho
✔ Search screen / report-like logic ho
✔ Reusable generic code banana ho

---

# 🏢 Real Project Example #1

### 🎯 Requirement:

> User filter choose kare:

* Industry (optional)
* Rating (optional)

---

## ❌ Galat approach (Multiple static queries)

```apex
if(industry != null && rating != null) {
    // query 1
} else if(industry != null) {
    // query 2
}
```

❌ Bad, unreadable, unscalable

---

## ✅ Dynamic SOQL – Industry Standard Way

```apex
String baseQuery = 'SELECT Id, Name, Industry, Rating FROM Account';
List<String> conditions = new List<String>();

if (industry != null) {
    conditions.add('Industry = :industry');
}

if (rating != null) {
    conditions.add('Rating = :rating');
}

if (!conditions.isEmpty()) {
    baseQuery += ' WHERE ' + String.join(conditions, ' AND ');
}

List<Account> accList = Database.query(baseQuery);
```

📌 Clean
📌 Scalable
📌 Safe

---

# 🔐 VERY IMPORTANT – SOQL Injection se kaise bache?

## ❌ Dangerous code (NEVER DO THIS)

```apex
String q = 'SELECT Id FROM Account WHERE Name = \'' + userInput + '\'';
Database.query(q);
```

🚨 Hacker input de sakta hai:

```
' OR Name != ''
```

---

## ✅ Safe Way (Bind Variables)

```apex
String q = 'SELECT Id FROM Account WHERE Name = :accName';
Database.query(q);
```

✔ Secure
✔ Industry standard

---

# 🏗️ Dynamic Object + Field Query (Advanced)

### 🎯 Requirement:

* Object name dynamic
* Field name dynamic

```apex
String objName = 'Account';
String fieldName = 'Industry';

String q = 'SELECT Id, Name FROM ' + objName +
           ' WHERE ' + fieldName + ' = :value';

List<SObject> result = Database.query(q);
```

⚠️ NOTE:

* Object/Field names **bind nahi hote**
* Sirf values bind hoti hain

---

# 🧪 Real Project Example #2 – Search Screen

```apex
public static List<Account> searchAccounts(String keyword) {

    String q = 'SELECT Id, Name FROM Account WHERE Name LIKE :searchKey';
    String searchKey = '%' + keyword + '%';

    return Database.query(q);
}
```

---

# 🔁 Dynamic SOQL with LIMIT / ORDER BY

```apex
String q = 'SELECT Id, Name FROM Account ORDER BY CreatedDate DESC LIMIT 10';
Database.query(q);
```

---

# ⚖️ Static vs Dynamic SOQL

| Feature            | Static   | Dynamic        |
| ------------------ | -------- | -------------- |
| Compile-time check | ✅ Yes    | ❌ No           |
| Security           | ✅ High   | ⚠️ Care needed |
| Flexibility        | ❌ Low    | ✅ High         |
| Performance        | ✅ Better | ⚠️ Slight cost |
| Readability        | ✅ Better | ❌ Medium       |

---

# 🏆 Industry Best Practices (MUST FOLLOW)

✔ Use Dynamic SOQL **only when needed**
✔ Always use **bind variables**
✔ Never trust user input
✔ Avoid in triggers if possible
✔ Log query in debug while dev
✔ Use `WITH SECURITY_ENFORCED` if needed

---

## Example with Security Enforced

```apex
String q = 'SELECT Id, Name FROM Account WHERE Industry = :industry WITH SECURITY_ENFORCED';
Database.query(q);
```

---

# 📌 Interview-Ready One-Liners

* “Dynamic SOQL is used when query structure is decided at runtime.”
* “Bind variables prevent SOQL injection.”
* “Static SOQL is preferred whenever possible.”
* “Dynamic SOQL sacrifices compile-time safety for flexibility.”

