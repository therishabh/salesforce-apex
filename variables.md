
# Variables kya hote hain (Apex me)

### Variable ka matlab

**Variable** ek container hota hai jisme hum **data store** karte hain, taaki use baad me use / modify kar saken.

📌 Apex **strongly typed language** hai
➡️ Matlab variable declare karte time **data type batana mandatory** hai.

---

## 🔹 Variable declare ka syntax (Apex)

```apex
DataType variableName;
```

Example:

```apex
Integer age;
String name;
```

Value assign karna:

```apex
Integer age = 25;
String name = 'Rishabh';
```

---

# Primitive vs Non-Primitive Data Types

## 🟢 Primitive Data Types

👉 Ye **simple data store** karte hain
👉 **Single value** hoti hai
👉 Memory kam use hoti hai
👉 Value directly store hoti hai

### Common Primitive Types in Apex

| Type     | Example                         |
| -------- | ------------------------------- |
| Integer  | `Integer count = 10;`           |
| Long     | `Long mobile = 9876543210L;`    |
| Double   | `Double price = 99.99;`         |
| Boolean  | `Boolean isActive = true;`      |
| String   | `String city = 'Delhi';`        |
| Date     | `Date d = Date.today();`        |
| Datetime | `Datetime dt = Datetime.now();` |
| Decimal  | `Decimal salary = 50000.50;`    |

📌 **Example**

```apex
Integer marks = 80;
Boolean passed = true;
```

---

## 🔵 Non-Primitive (Reference) Data Types

👉 **Complex data** store karte hain
👉 **Multiple values / structure** ho sakta hai
👉 Reference store hota hai (actual data memory me alag hota hai)

### Common Non-Primitive Types

| Type   | Use                |
| ------ | ------------------ |
| List   | Ordered collection |
| Set    | Unique values      |
| Map    | Key-Value pair     |
| Object | Salesforce records |
| Class  | Custom logic       |

### Example: List

```apex
List<String> names = new List<String>();
names.add('Amit');
names.add('Ravi');
```

### Example: Map

```apex
Map<Id, String> studentMap = new Map<Id, String>();
```

### Example: SObject

```apex
Account acc = new Account();
acc.Name = 'Test Account';
```

📌 **Difference yaad rakhne ka trick**

* Primitive = **Value**
* Non-Primitive = **Reference**
