
# 🧱 Constructor in Salesforce (Apex)

## Constructor kya hota hai? (Simple language)

> **Constructor ek special method hota hai jo
> object create hote hi automatically execute hota hai**

📌 Matlab:

* `new ClassName()` likhte hi
* Constructor call hota hai
* Object ko **initial state** deta hai

---

## 2️⃣ Constructor ke Rules (VERY IMPORTANT)

✔ Constructor ka **naam class ke naam jaisa hota hai**
✔ Constructor ka **koi return type nahi hota**
✔ Constructor **automatically call hota hai**
✔ Ek class me **multiple constructors** ho sakte hain

---

## Basic Constructor Example

```apex
public class Student {

    Integer marks;

    // Constructor
    public Student() {
        System.debug('Constructor called');
        marks = 50;
    }
}
```

### Usage

```apex
Student s = new Student();
System.debug(s.marks);
```

### Output

```
Constructor called
50
```

---

## Parameterized Constructor

```apex
public class Student {

    Integer marks;

    public Student(Integer m) {
        marks = m;
    }
}
```

### Usage

```apex
Student s = new Student(80);
System.debug(s.marks);
```

---

## Constructor Overloading

> **Same constructor name
> but different parameters**

```apex
public class Student {

    Integer marks;

    public Student() {
        marks = 40;
    }

    public Student(Integer m) {
        marks = m;
    }
}
```

📌 Salesforce automatically decide karta hai kaunsa constructor call karna hai

---

##  Constructor Chaining (`this()`)

> Ek constructor se doosra constructor call karna

```apex
public class Student {

    Integer marks;

    public Student() {
        this(50); // calls parameterized constructor
    }

    public Student(Integer m) {
        marks = m;
    }
}
```

📌 `this()` hamesha constructor ki **first line** hoti hai

---

## Constructor + `super()` (Inheritance)

### Parent Class

```apex
public class Person {

    public Person() {
        System.debug('Person constructor');
    }
}
```

### Child Class

```apex
public class Student extends Person {

    public Student() {
        super(); // parent constructor
        System.debug('Student constructor');
    }
}
```

### Output

```
Person constructor
Student constructor
```

📌 Agar `super()` explicitly nahi likhte:
➡️ Salesforce **automatically** call karta hai

---

## Real Salesforce Use Cases

---

### 🔹 1. Service Class Initialization

```apex
public class AccountService {

    private Account acc;

    public AccountService(Account a) {
        acc = a;
    }

    public void updateName() {
        acc.Name = acc.Name + ' - Updated';
    }
}
```

---

### 🔹 2. Wrapper Class

```apex
public class AccountWrapper {

    public Account acc;
    public Boolean isPremium;

    public AccountWrapper(Account a) {
        acc = a;
        isPremium = a.AnnualRevenue > 100000;
    }
}
```

---

## Constructor vs Method (Confusion Clear)

| Point       | Constructor       | Method         |
| ----------- | ----------------- | -------------- |
| Name        | Same as class     | Any name       |
| Return type | ❌ None            | ✔ Required     |
| Call        | Auto              | Manual         |
| Purpose     | Initialize object | Perform action |

---

## Common Mistakes (Avoid These)

❌ Constructor me DML / SOQL heavy logic
❌ Business logic constructor me likhna
❌ Static logic constructor me rakhna

---

## 🔥 Interview Ready One-Liners

> A constructor in Salesforce Apex is a special method that initializes an object and is automatically invoked when an instance of the class is created.

> Constructors are commonly used in service and wrapper classes to set up required data before executing business logic.

---

## 🧠 Ultimate Memory Trick (Never Forget)

> **Class ka naam + no return = Constructor**
> **Object bana = Constructor chala**

---
# 🔹 Instance Block in Salesforce (Apex)
---
## 1️⃣ Instance Block kya hota hai? (Simple language)

> **Instance Block ek special code block hota hai jo
> object create hote hi automatically execute hota hai**

📌 Matlab:

* `new ClassName()` likhte hi
* **Constructor se pehle**
* Instance block run hota hai

---

## 2️⃣ Instance Block ka Syntax

```apex
{
    // initialization code
}
```

👉 Iske:

* ❌ koi naam nahi hota
* ❌ koi return type nahi hota
* ❌ koi keyword (public/private) nahi hota

---

## 3️⃣ Instance Block vs Constructor (Big Confusion Clear)

| Point            | Instance Block        | Constructor            |
| ---------------- | --------------------- | ---------------------- |
| Kab run hota hai | Object create hote hi | Instance block ke baad |
| Name             | ❌ No name             | ✔ Class name           |
| Parameters       | ❌ No                  | ✔ Yes                  |
| Overloading      | ❌ No                  | ✔ Yes                  |
| Purpose          | Common initialization | Custom initialization  |

---

## 4️⃣ Simple Example (Step-by-Step)

```apex
public class Student {

    Integer marks;

    // 🔹 Instance Block
    {
        System.debug('Instance block executed');
        marks = 50;
    }

    // 🔹 Constructor
    public Student() {
        System.debug('Constructor executed');
    }
}
```

### Usage

```apex
Student s = new Student();
System.debug(s.marks);
```

### Output Order

```
Instance block executed
Constructor executed
50
```

📌 **Important**
➡️ Instance block **hamesha constructor se pehle** run hota hai

---

## 5️⃣ Real Use Case: Default Initialization

### 🎯 Scenario

* Har Student ka default marks = 40
* Chahe kaunsa bhi constructor call ho

---

### ❌ Without Instance Block (Duplication)

```apex
public Student() {
    marks = 40;
}

public Student(Integer m) {
    marks = m;
}
```

---

### ✅ With Instance Block (Best Practice)

```apex
public class Student {

    Integer marks;

    // 🔹 Common initialization
    {
        marks = 40;
    }

    public Student() {
        // nothing
    }

    public Student(Integer m) {
        marks = m;
    }
}
```

📌 Default logic **ek hi jagah**


## 🔥 Interview Ready Lines

> An instance block in Apex is used for common initialization logic that executes automatically every time an object is created, before the constructor is invoked.

> Instance blocks help avoid duplication when multiple constructors require the same initialization logic.

---
# 🔹 Static Block in Salesforce

---

## 1️⃣ Static Block kya hota hai? (Simple language)

> **Static Block ek special code block hota hai jo
> class load hote hi automatically execute ho jata hai**

📌 Matlab:

* Jab class **memory me load hoti hai**
* **Object banane se pehle**
* Static block **sirf EK BAAR** run hota hai

---

## 2️⃣ Static Block ka Syntax

```apex
static {
    // initialization logic
}
```

👉 Iske:

* ❌ koi naam nahi hota
* ❌ koi return type nahi hota
* ❌ parameters nahi hote

---

## 3️⃣ Simple Static Block Example

```apex
public class ConfigService {

    static Integer tax;

    // 🔹 Static Block
    static {
        System.debug('Static block executed');
        tax = 18;
    }
}
```

📌 Jab bhi class **first time reference** hogi:

```apex
System.debug(ConfigService.tax);
```

### Output

```
Static block executed
18
```

📌 **Important**
➡️ Object banaya ho ya nahi, static block run ho jata hai

---

## 6️⃣ Order of Execution (Static Block Context)

```apex
public class Demo {

    static {
        System.debug('Static block');
    }

    {
        System.debug('Instance block');
    }

    public Demo() {
        System.debug('Constructor');
    }
}
```

### Execution

```apex
Demo d = new Demo();
```

### Output Order

```
Static block
Instance block
Constructor
```

📌 **Memory trick**

> **Static → Instance → Constructor**

---

## 7️⃣ Real Salesforce Use Case (VERY IMPORTANT)

### 🎯 Scenario: Prevent Trigger Recursion

```apex
public class TriggerHelper {

    public static Boolean isFirstRun;

    static {
        isFirstRun = true;
    }
}
```

### Trigger

```apex
trigger AccountTrigger on Account (before update) {

    if(TriggerHelper.isFirstRun) {
        TriggerHelper.isFirstRun = false;

        // logic executes only once
    }
}
```

📌 Static block ensures:

* Flag initialized once per transaction
* Recursion avoided

---


## 🔥 Interview Ready One-Liners

> A static block in Apex executes once when the class is loaded into memory and is mainly used to initialize static variables or configuration data.

> Static blocks run before instance blocks and constructors.

---
