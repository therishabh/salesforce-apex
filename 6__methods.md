
# Methods

---

## 1️⃣ Method kya hota hai?

### Simple definition

> **Method ek block of code hota hai jo ek specific kaam karta hai**

👉 Jaise:

* Calculation
* Data validate karna
* Record insert/update karna
* Value return karna

📌 Apex me **business logic mostly methods ke andar hi hota hai**

---

## 2️⃣ Method ka Basic Syntax (Apex)

```apex
accessModifier returnType methodName(parameters) {
    // logic
}
```

---

### 🔹 Example

```apex
public Integer addNumbers(Integer a, Integer b) {
    return a + b;
}
```

---

## 3️⃣ Method ke Parts (Line-by-Line Meaning)

```apex
public        // Access Modifier – method ka access level
Integer       // Return type – kya value return hogi
addNumbers    // Method name
(Integer a, Integer b) // Parameters (inputs)
{
    return a + b; // Method body (logic)
}
```

---

## 4️⃣ Return Type ke Types

### 🔹 1. Method jo value return karta hai

```apex
public Integer getMarks() {
    return 90;
}
```

---

### 🔹 2. Method jo kuch return nahi karta (`void`)

```apex
public void printMessage() {
    System.debug('Hello Salesforce');
}
```

📌 `void` ka matlab = **no return**

---

## 5️⃣ Parameters kya hote hain?

👉 Parameters = method ko data dena

```apex
public void setName(String name) {
    System.debug(name);
}
```

Method call:

```apex
setName('Rishabh');
```

---

## 6️⃣ Types of Methods in Salesforce

---

## 🔹 1. Instance Method

👉 Object ke sath call hota hai

```apex
public class Student {

    public Integer marks;

    public Integer getMarks() {
        return marks;
    }
}
```

Usage:

```apex
Student s = new Student();
s.marks = 80;
System.debug(s.getMarks());
```

---

## 🔹 2. Static Method

👉 Class ke naam se call hota hai
👉 Object banana zaroori nahi

```apex
public class MathUtil {

    public static Integer square(Integer x) {
        return x * x;
    }
}
```

Usage:

```apex
System.debug(MathUtil.square(5));
```

---

## 7️⃣ Getter & Setter Methods

👉 Encapsulation ka core part

```apex
public class Student {

    private Integer marks;

    public void setMarks(Integer value) {
        marks = value;
    }

    public Integer getMarks() {
        return marks;
    }
}
```

---

## 8️⃣ Method Overloading (Same name, different params)

```apex
public class Calculator {

    public Integer add(Integer a, Integer b) {
        return a + b;
    }

    public Decimal add(Decimal a, Decimal b) {
        return a + b;
    }
}
```

📌 Rule:

* Method name same
* Parameters different

---

## 9️⃣ Method Overriding (Inheritance required)

```apex
public virtual class Parent {

    public virtual void show() {
        System.debug('Parent');
    }
}
```

```apex
public class Child extends Parent {

    public override void show() {
        System.debug('Child');
    }
}
```

---

## 🔥 Static vs Instance Method (IMPORTANT)

| Point   | Static Method       | Instance Method       |
| ------- | ------------------- | --------------------- |
| Call    | Class name          | Object                |
| Memory  | One copy            | Per object            |
| Use     | Utility logic       | Object specific logic |
| Example | `MathUtil.square()` | `student.getMarks()`  |

---

## 🔑 Interview One-Liners

> Methods in Apex are used to encapsulate reusable business logic and can be instance or static depending on the use case.

> Static methods are commonly used in utility and service classes, while instance methods operate on object-specific data.

---
