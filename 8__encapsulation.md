# 🔐 Encapsulation in Salesforce (Apex) – COMPLETE NOTES

## 1️⃣ Encapsulation kya hota hai? (Simple words me)

**Encapsulation = Data ko protect karna + controlled access dena**

👉 Matlab:

* Data (**variables**) ko **direct access se hide** kar dena
* Access sirf **methods (getter / setter)** ke through dena

📌 Salesforce me Encapsulation **bahut important** hai kyunki:

* Data security
* Business rules enforce karna
* Clean & maintainable code

---

## 2️⃣ Encapsulation ka Golden Rule

> **Variables → private**
> **Access → public methods (getter / setter)**

---

## 3️⃣ Real Salesforce Scenario (Student Example)

### 🎯 Scenario

* Student ke **marks** hain
* Koi bhi directly marks **set nahi kar sakta**
* Marks **0–100 ke beech hi hone chahiye**

➡️ Ye rule **Encapsulation ke bina possible nahi**

---

## 4️⃣ ❌ Without Encapsulation (Wrong Approach)

```apex
public class Student {

    public Integer marks;
}
```

❌ Problem:

```apex
Student s = new Student();
s.marks = 500;   // ❌ Invalid but allowed
```

👉 Data unsafe
👉 Business rule broken

---

## 5️⃣ ✅ With Encapsulation (CORRECT Salesforce Way)

### 🔥 Fully Encapsulated Apex Class (Line-by-Line Explained)

```apex
public class Student {

    // 🔒 PRIVATE VARIABLE
    // Is variable ko directly bahar se access nahi kar sakte
    // Ye Encapsulation ka core part hai
    private Integer marks;

    // ✅ PUBLIC SETTER METHOD
    // Iske through hi marks set kiye ja sakte hain
    public void setMarks(Integer inputMarks) {

        // 🔐 Business rule applied
        // Marks 0 se kam ya 100 se zyada nahi hone chahiye
        if(inputMarks >= 0 && inputMarks <= 100) {
            marks = inputMarks;
        } else {
            // Invalid input hone par exception
            throw new IllegalArgumentException(
                'Marks must be between 0 and 100'
            );
        }
    }

    // ✅ PUBLIC GETTER METHOD
    // Private variable ki value safely return karta hai
    public Integer getMarks() {
        return marks;
    }
}
```

---

## 6️⃣ Class ko Use Karna (Encapsulation in Action)

```apex
Student s = new Student();

// ✅ Valid input
s.setMarks(85);
System.debug(s.getMarks()); // Output: 85

// ❌ Invalid input
s.setMarks(150); // Exception throw karega
```

📌 **Key Point**
➡️ User chahe jo kare, **rules break nahi honge**

---

## 7️⃣ Salesforce Real-World Example (SObject + Encapsulation)

### 🎯 Scenario

* Account ka **AnnualRevenue**
* Negative value **allowed nahi**

---

### 🔥 Encapsulated Service Class

```apex
public with sharing class AccountService {

    // Private SObject reference
    private Account acc;

    // Constructor
    public AccountService(Account inputAcc) {
        acc = inputAcc;
    }

    // Setter with validation
    public void setAnnualRevenue(Decimal revenue) {

        if(revenue < 0) {
            throw new IllegalArgumentException(
                'Annual Revenue cannot be negative'
            );
        }

        acc.AnnualRevenue = revenue;
    }

    // Getter
    public Decimal getAnnualRevenue() {
        return acc.AnnualRevenue;
    }
}
```

---

### 🔁 Usage

```apex
Account a = new Account(Name = 'Test Account');
AccountService service = new AccountService(a);

service.setAnnualRevenue(500000);
insert a;
```

---

## 8️⃣ Encapsulation ke Benefits (Salesforce POV)

| Benefit          | Explanation            |
| ---------------- | ---------------------- |
| 🔐 Data Security | Direct access blocked  |
| 📏 Validation    | Business rules enforce |
| 🧹 Clean Code    | Logic centralized      |
| 🔄 Easy Changes  | Rule change = ek jagah |
| 🧪 Testable      | Unit testing easy      |

---

## 9️⃣ Encapsulation + Access Modifiers Relation

| Component       | Modifier        |
| --------------- | --------------- |
| Variable        | `private`       |
| Getter / Setter | `public`        |
| Logic           | Method ke andar |

---

## 🔑 Interview Ready Definition (Yaad Rakhna)

> Encapsulation is the process of hiding sensitive data using private variables and providing controlled access through public methods to enforce business rules and data security.

---

## 🧠 One-Line Salesforce Example Summary

> In Salesforce Apex, Encapsulation is commonly implemented by keeping SObject fields private inside service classes and exposing them through validated getter and setter methods.

---


Perfect 👍
Ab hum **Getter & Setter ka SHORTCUT syntax** (Apex Property) detail me samajhenge — **ye Salesforce ka ek bahut powerful + clean feature hai** aur **interview me bhi bahut poocha jata hai**.

---

# ⚡ Getter & Setter Shortcut Syntax in Salesforce (Apex Properties)

## 1️⃣ Normal Getter & Setter (Old / Long Way)

Pehle yaad karo normal encapsulation ka tarika 👇

```apex
public class Student {

    private Integer marks;

    public void setMarks(Integer m) {
        if(m >= 0 && m <= 100) {
            marks = m;
        }
    }

    public Integer getMarks() {
        return marks;
    }
}
```

❌ Code thoda **lengthy**
❌ Har variable ke liye 2 methods likhne padte hain

---

## 2️⃣ ✅ Shortcut Syntax (Apex Property)

Salesforce Apex me **property** ka concept hota hai
➡️ Jo **getter + setter ko ek hi line me combine** kar deta hai

---

### 🔹 Basic Property Syntax

```apex
public Integer marks { get; set; }
```

📌 Iska matlab:

* `get` → value return karega
* `set` → value assign karega
* Compiler **automatically methods bana deta hai**

---

## 3️⃣ Encapsulation ke sath Property (BEST PRACTICE)

### 🔥 Fully Encapsulated Property Example

```apex
public class Student {

    private Integer _marks;

    public Integer marks {
        get {
            return _marks;
        }
        set {
            if(value >= 0 && value <= 100) {
                _marks = value;
            } else {
                throw new IllegalArgumentException(
                    'Marks must be between 0 and 100'
                );
            }
        }
    }
}
```

---

## 4️⃣ Important Keywords (Property ke andar)

### 🔑 `value` keyword

* `value` = setter me aane wali value
* Automatically available hota hai

```apex
set {
    _marks = value;
}
```

---

## 5️⃣ Use Karna (Looks like variable, works like method)

```apex
Student s = new Student();

// Setter call ho raha hai (method internally)
s.marks = 85;

// Getter call ho raha hai
System.debug(s.marks);
```

📌 **Important**
➡️ Syntax variable jaisa lagta hai
➡️ But internally method call hota hai

---

## 6️⃣ Read-Only / Write-Only Properties

### 🔒 Read-only (Only GET)

```apex
public Integer totalStudents {
    get;
    private set;
}
```

✔ Bahar se sirf read
❌ Set sirf class ke andar

---

### ✍️ Write-only (Rare case)

```apex
public Integer secretCode {
    private get;
    set;
}
```

---

## 7️⃣ Property + Constructor Example

```apex
public class Student {

    public String name { get; private set; }
    public Integer marks { get; set; }

    public Student(String n, Integer m) {
        name = n;
        marks = m;
    }
}
```

---

## 8️⃣ Salesforce Real Example (Account Wrapper)

```apex
public with sharing class AccountWrapper {

    private Account acc;

    public Decimal annualRevenue {
        get {
            return acc.AnnualRevenue;
        }
        set {
            if(value < 0) {
                throw new IllegalArgumentException(
                    'Revenue cannot be negative'
                );
            }
            acc.AnnualRevenue = value;
        }
    }

    public AccountWrapper(Account a) {
        acc = a;
    }
}
```

---

## 9️⃣ When to Use Which?

| Situation         | Use                    |
| ----------------- | ---------------------- |
| Simple variable   | `get; set;`            |
| Validation needed | Custom get / set block |
| Read-only         | `get; private set;`    |
| Encapsulation     | Property preferred     |

---

## 🔑 Interview One-Liners

> Apex Properties provide a shortcut syntax to implement encapsulation by combining getter and setter logic in a single block.

> Although properties are accessed like variables, they execute getter and setter methods internally.

---

## 🧠 Memory Trick

> **Property = Variable jaisa use, Method jaisa kaam**

---
