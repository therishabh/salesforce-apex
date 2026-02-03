
# Classes kya hoti hain (Apex)

### Class ka matlab

**Class ek blueprint hoti hai**
➡️ Variables + Methods ka collection

👉 Apex me **saara logic classes ke andar hi likha jata hai**

---

## 🔹 Class declare ka syntax

```apex
accessModifier class ClassName {
    // variables
    // methods
}
```

### Simple Example

```apex
public class Student {

    String name;
    Integer marks;

    public void display() {
        System.debug('Name: ' + name);
        System.debug('Marks: ' + marks);
    }
}
```

---

## 🔹 Class ka use (Object banana)

```apex
Student s = new Student();
s.name = 'Rahul';
s.marks = 85;
s.display();
```

📌 **Important**

* Class = Blueprint
* Object = Real instance

---

# Access Modifier vs Access Specifier (IMPORTANT INTERVIEW TOPIC)

Salesforce me log aksar confuse ho jaate hain, isliye dhyaan se padho 👇

---

## 🟡 Access Modifier kya hota hai?

👉 Access Modifier decide karta hai ki
**Class / Variable / Method ko kahaan tak access kiya ja sakta hai**

### Apex Access Modifiers

| Modifier  | Access                              |
| --------- | ----------------------------------- |
| private   | Sirf same class                     |
| public    | Same org ke andar                   |
| protected | Same class + child class            |
| global    | Sab jagah (managed package ke liye) |

### Example

```apex
public class Demo {
    private Integer a;
    public Integer b;
}
```

---

## 🟣 Access Specifier kya hota hai?

👉 **Salesforce ke context me**
Access Specifier ka matlab hota hai **extra keywords** jo access behavior ko modify karte hain

### Common Access Specifiers in Apex

| Specifier       | Use                          |
| --------------- | ---------------------------- |
| static          | Class level access           |
| final           | Change nahi ho sakta         |
| transient       | Serialization ke time ignore |
| with sharing    | Sharing rules follow         |
| without sharing | Sharing rules ignore         |

📌 Example:

```apex
public with sharing class AccountService {
}
```

```apex
public static Integer count = 0;
```

---

## 🔥 Access Modifier vs Access Specifier (Clear Difference)

| Point            | Access Modifier    | Access Specifier |
| ---------------- | ------------------ | ---------------- |
| Purpose          | Visibility control | Behavior control |
| Example          | public, private    | static, final    |
| Decide karta hai | Kaun access kare   | Kaise access ho  |
| Mandatory        | Haan               | Nahi             |

📌 **Interview line yaad rakhna**

> Access Modifier batata hai *who can access*
> Access Specifier batata hai *how it behaves*

---

## Combined Example (Class + Variable + Modifier)

```apex
public with sharing class StudentService {

    public static final String SCHOOL = 'DPS';

    private Integer totalStudents;

    public StudentService() {
        totalStudents = 100;
    }

    public Integer getTotalStudents() {
        return totalStudents;
    }
}
```

---

## 🔥 Fully Commented Apex Example (Line-by-Line Explanation)

```apex
// 'public'  → Access Modifier
//            Is class ko poore Salesforce org me access kiya ja sakta hai
//
// 'with sharing' → Access Specifier
//                  Ye ensure karta hai ki current user ke
//                  sharing rules & security follow hon
//
// 'class StudentService' → Class declaration
//                          Class ek blueprint hoti hai jo
//                          variables + methods ko contain karti hai
public with sharing class StudentService {

    // 'public' → Ye variable class ke bahar se bhi access ho sakta hai
    //
    // 'static' → Ye class-level variable hai
    //            Iska sirf EK hi copy hota hai poore org me,
    //            object banane ki zarurat nahi hoti
    //
    // 'final'  → Iski value change nahi ho sakti
    //            (constant variable ban jata hai)
    //
    // 'String' → Primitive data type
    //
    // 'SCHOOL' → Constant variable ka naam
    //
    // 'DPS'    → Fixed value assigned at compile time
    //
    // 📌 Use case:
    //    Jab koi common value har jagah same rahe
    //    (jaise School Name, Country Code, etc.)
    public static final String SCHOOL = 'DPS';

    // 'private' → Ye variable sirf isi class ke andar accessible hai
    //              Bahar se directly access nahi ho sakta
    //
    // 'Integer' → Primitive data type
    //
    // 'totalStudents' → Instance variable
    //                   Har object ke liye alag value ho sakti hai
    //
    // 📌 Security reason:
    //    Direct access band karke,
    //    getter method ke through value dena (best practice)
    private Integer totalStudents;

    // Constructor
    //
    // Constructor ka naam hamesha class ke naam jaisa hota hai
    //
    // Ye automatically call hota hai jab
    // 'new StudentService()' likha jata hai
    //
    // 📌 Use:
    //    Object create hote hi initial setup karne ke liye
    public StudentService() {

        // totalStudents variable ko initial value assign kar rahe hain
        //
        // Matlab:
        // Jab bhi StudentService ka object banega,
        // totalStudents by default 100 ho jayega
        totalStudents = 100;
    }

    // 'public' → Method ko class ke bahar se call kiya ja sakta hai
    //
    // 'Integer' → Return type
    //              Ye method Integer value return karega
    //
    // 'getTotalStudents()' → Getter method
    //
    // 📌 Getter ka use:
    //    private variable ki value safely bahar dena
    public Integer getTotalStudents() {

        // totalStudents ki current value return kar raha hai
        return totalStudents;
    }
}
```

---

## 🧠 Sirf Is Example Se Jo Concepts Clear Ho Jaate Hain

### ✅ Access Modifier

* `public` → Class / method bahar se accessible
* `private` → Variable sirf class ke andar

### ✅ Access Specifier

* `with sharing` → User ke sharing rules follow honge
* `static` → Class-level member
* `final` → Value change nahi hogi

### ✅ Variable Types

* `SCHOOL` → **Static + Final (Constant)**
* `totalStudents` → **Instance variable**

### ✅ Constructor

* Object create hote hi auto-call
* Initial values set karta hai

### ✅ Best Practice

* Variables → `private`
* Access → `public getter method`

---

## 🔁 Object Create & Use (Samajhne ke liye)

```apex
StudentService service = new StudentService();

// Static variable (class ke naam se)
System.debug(StudentService.SCHOOL);

// Instance variable value getter se
System.debug(service.getTotalStudents());
```

---

## 📌 Interview Ready One-Line Summary

> This class demonstrates the use of access modifiers, access specifiers, static final constants, constructors, and encapsulation using private variables with public getter methods.

---
