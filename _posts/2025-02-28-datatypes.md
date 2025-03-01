---
layout: default
title: "Java Datatypes"
date: 2025-02-28
---

Important Data type convertions that we should remember for OCP
## **Comprehensive Guide on Method Overloading with Primitive and Wrapper Classes in Java**
Understanding how Java resolves overloaded methods when dealing with **primitive types**, **wrapper classes**, **widening**, **autoboxing**, and **varargs** is crucial for the **OCP (Oracle Certified Professional) Java Exam**. Below is a **detailed explanation with examples** covering **all scenarios**.

---

# **1. Exact Match is Preferred**
When an exact match for the argument type exists, Java selects that method.

## **Example:**
```java
public class OverloadingExample {
    public static void test(int x) { 
        System.out.println("Primitive int method called");
    }
    
    public static void test(Integer x) { 
        System.out.println("Wrapper Integer method called");
    }

    public static void main(String[] args) {
        test(10);      // Calls primitive int method
        test(Integer.valueOf(20)); // Calls wrapper Integer method
    }
}
```
## **Output:**
```
Primitive int method called
Wrapper Integer method called
```

## **Rule:**
- **If both primitive and wrapper methods exist, the primitive version is preferred when passing a primitive.**
- **If an exact wrapper match exists, it is selected for wrapper arguments.**

---

# **2. Primitive is Preferred Over Wrapper Class**
When both **primitive** and **wrapper class** versions of a method exist, and a **primitive argument** is passed, Java **chooses the primitive method**.

## **Example:**
```java
public class OverloadingExample {
    public static void test(int x) { 
        System.out.println("Primitive int method called");
    }
    
    public static void test(Integer x) { 
        System.out.println("Wrapper Integer method called");
    }

    public static void main(String[] args) {
        int a = 10;
        test(a); // Calls test(int), NOT test(Integer)
    }
}
```
## **Output:**
```
Primitive int method called
```

---

# **3. Widening is Preferred Over Autoboxing**
If no exact match is found, Java **prefers widening a primitive** over **autoboxing**.

## **Example:**
```java
public class OverloadingExample {
    public static void test(long x) { // int → long (Widening)
        System.out.println("Widening to long method called");
    }
    
    public static void test(Integer x) { // int → Integer (Autoboxing)
        System.out.println("Autoboxing to Integer method called");
    }

    public static void main(String[] args) {
        int a = 10;
        test(a); // Widening happens, NOT Autoboxing
    }
}
```
## **Output:**
```
Widening to long method called
```

## **Rule:**
- Widening (int → long) **is preferred** over autoboxing (int → Integer).

---

# **4. Autoboxing is Preferred Over Varargs**
When both **autoboxing** and **varargs** methods exist, Java prefers **autoboxing** over **varargs**.

## **Example:**
```java
public class OverloadingExample {
    public static void test(Integer x) { // Autoboxing int → Integer
        System.out.println("Autoboxing to Integer method called");
    }

    public static void test(int... x) { // Varargs method
        System.out.println("Varargs method called");
    }

    public static void main(String[] args) {
        int a = 10;
        test(a); // Autoboxing is preferred
    }
}
```
## **Output:**
```
Autoboxing to Integer method called
```

## **Rule:**
- **Autoboxing (int → Integer) is preferred over varargs (int → int...)**.

---

# **5. Widening is Preferred Over Varargs**
If both **widening** and **varargs** methods exist, Java chooses **widening**.

## **Example:**
```java
public class OverloadingExample {
    public static void test(long x) { // Widening int → long
        System.out.println("Widening to long method called");
    }

    public static void test(int... x) { // Varargs method
        System.out.println("Varargs method called");
    }

    public static void main(String[] args) {
        int a = 10;
        test(a); // Widening is preferred
    }
}
```
## **Output:**
```
Widening to long method called
```

## **Rule:**
- **Widening is preferred over varargs.**

---

# **6. Autoboxing Followed by Widening is NOT Allowed**
Java does **not** allow **autoboxing followed by widening**.

## **Example:**
```java
public class OverloadingExample {
    public static void test(long x) {
        System.out.println("Widening to long method called");
    }

    public static void main(String[] args) {
        Integer a = 10;
        // test(a); // Compilation error: Integer cannot be widened to long
    }
}
```
## **Rule:**
🚫 **Autoboxing (Integer) followed by Widening (Integer → long) is NOT allowed.**

---

# **7. Varargs is the Last Resort**
If no other method is applicable, **varargs will be used**.

## **Example:**
```java
public class OverloadingExample {
    public static void test(String x) { 
        System.out.println("String method called");
    }

    public static void test(Object... x) { // Varargs
        System.out.println("Varargs method called");
    }

    public static void main(String[] args) {
        test(10); // No exact, widening, or autoboxing match, so varargs is used
    }
}
```
## **Output:**
```
Varargs method called
```

---

# **8. What Happens When Passing `null`?**
When passing `null`, Java chooses the **most specific** method.

## **Example:**
```java
public class OverloadingExample {
    public static void test(Object x) {
        System.out.println("Object method called");
    }

    public static void test(String x) {
        System.out.println("String method called");
    }

    public static void main(String[] args) {
        test(null); // String is more specific than Object
    }
}
```
## **Output:**
```
String method called
```

🚫 **If two equally specific methods exist (e.g., `Integer` and `Double`), a compilation error occurs.**

---

# **Summary of Method Resolution Order**
| Case | Preferred Method |
|------|----------------|
| **Exact match** | **Chosen first** |
| **Primitive vs. Wrapper** | **Primitive is preferred** |
| **Widening vs. Autoboxing** | **Widening is preferred** |
| **Autoboxing vs. Varargs** | **Autoboxing is preferred** |
| **Widening vs. Varargs** | **Widening is preferred** |
| **Autoboxing + Widening** | **NOT allowed** |
| **Varargs** | **Last resort** |
| **Null Argument** | **Most specific method chosen** |

---

# **Conclusion**
- **Primitive types are always preferred over wrapper classes.**
- **Widening beats autoboxing.**
- **Autoboxing beats varargs.**
- **Autoboxing followed by widening is not allowed.**
- **Null resolves to the most specific method.**
- **Varargs is used only when no other match exists.**
