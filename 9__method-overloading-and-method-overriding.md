# 🔁 Method Overloading vs Method Overriding 

---

## 🟢 PART 1: METHOD OVERLOADING

---

## 1️⃣ Method Overloading kya hota hai?

> **Same class ke andar SAME method name
> but DIFFERENT parameters**

👉 Matlab:

* Method ka **naam same**
* Parameters **different** (number / type)
* **Inheritance required nahi**

📌 Decision **compile time** par hota hai
➡️ Isliye ise **Compile Time Polymorphism** kehte hain

---

## 2️⃣ Method Overloading – Simple Example

```apex
public class Calculator {

    // Method 1
    public Integer add(Integer a, Integer b) {
        return a + b;
    }

    // Method 2 (Same name, different parameters)
    public Integer add(Integer a, Integer b, Integer c) {
        return a + b + c;
    }
}
```

### Usage

```apex
Calculator c = new Calculator();

System.debug(c.add(2, 3));       // Calls add(Integer, Integer)
System.debug(c.add(1, 2, 3));    // Calls add(Integer, Integer, Integer)
```

📌 Compiler khud samajh jata hai kaunsa method call karna hai.

---

## 3️⃣ Overloading ke Rules (IMPORTANT)

✅ Same class
✅ Same method name
✅ Parameters **must be different**

❌ Sirf return type change karke overloading **nahi hoti**

### ❌ INVALID Overloading Example

```apex
public Integer add(Integer a, Integer b) { }
public Decimal add(Integer a, Integer b) { } // ❌ NOT allowed
```

---

## 4️⃣ Salesforce Real Use Case (Overloading)

```apex
public class AccountService {

    public void createAccount(String name) {
        insert new Account(Name = name);
    }

    public void createAccount(String name, String industry) {
        insert new Account(Name = name, Industry = industry);
    }
}
```

📌 Same kaam, different inputs → **Perfect Overloading use**

---

## 🧠 One-Line Memory Trick (Overloading)

> **Same class + same name + different inputs**

---

---

## 🔵 PART 2: METHOD OVERRIDING

---

## 5️⃣ Method Overriding kya hota hai?

> **Parent class ke method ko
> Child class me REDEFINE karna**

👉 Matlab:

* **Inheritance required**
* Method name **same**
* Parameters **same**
* Child class apna implementation deta hai

📌 Decision **runtime** par hota hai
➡️ Isliye ise **Runtime Polymorphism** kehte hain

---

## 6️⃣ Method Overriding – Basic Example

### 🔹 Parent Class

```apex
public virtual class Parent {

    public virtual void show() {
        System.debug('This is Parent');
    }
}
```

### 🔹 Child Class

```apex
public class Child extends Parent {

    public override void show() {
        System.debug('This is Child');
    }
}
```

---

### Usage

```apex
Parent p = new Child();
p.show();
```

### Output

```
This is Child
```

📌 Object **Child ka hai**, isliye **Child ka method call hota hai**

---

## 7️⃣ Overriding ke Rules (VERY IMPORTANT)

✅ Inheritance must
✅ Parent method → `virtual`
✅ Child method → `override`
✅ Signature same hona chahiye

❌ Static methods override **nahi hote**
❌ Private methods override **nahi hote**

---

## 8️⃣ Salesforce Real Example (Overriding)

### 🔹 Abstract / Parent Class

```apex
public virtual class NotificationService {

    public virtual void send(String message) {
        System.debug('Sending notification');
    }
}
```

### 🔹 Child Class – Email

```apex
public class EmailNotification extends NotificationService {

    public override void send(String message) {
        System.debug('Sending Email: ' + message);
    }
}
```

### 🔹 Child Class – SMS

```apex
public class SMSNotification extends NotificationService {

    public override void send(String message) {
        System.debug('Sending SMS: ' + message);
    }
}
```

---

### Usage (Abstraction + Overriding)

```apex
NotificationService service;

service = new EmailNotification();
service.send('Hello');

service = new SMSNotification();
service.send('Hi');
```

📌 Same method → different behavior → **Overriding power**

---

## 🧠 One-Line Memory Trick (Overriding)

> **Parent ka method, child ka behaviour**

---

---

## 🔥 FINAL COMPARISON TABLE (INTERVIEW GOLD)

| Point          | Overloading  | Overriding    |
| -------------- | ------------ | ------------- |
| Class          | Same class   | Parent–Child  |
| Inheritance    | ❌ No         | ✅ Yes         |
| Method name    | Same         | Same          |
| Parameters     | Different    | Same          |
| Decision time  | Compile time | Runtime       |
| Polymorphism   | Compile-time | Runtime       |
| Static methods | ✅ Allowed    | ❌ Not allowed |

---

## 🎯 Common Interview Traps (Avoid These)

❓ *Can static methods be overridden?*
❌ No, static methods belong to class, not object

❓ *Is return type enough for overloading?*
❌ No, parameters must differ

❓ *Can private methods be overridden?*
❌ No, they are not visible to child class

---

## 🧠 Ultra-Easy Difference Trick

> **Overloading = SAME class, DIFFERENT input**
> **Overriding = DIFFERENT class, SAME input**

---
