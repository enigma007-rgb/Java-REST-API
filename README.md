# Java-REST-API

public class Main {
    public static void main(String[] args) {
        System.out.println("Hello, World!");
    }
}   

import java.util.Scanner;

public class SumCalculator {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        System.out.print("Enter the first number: ");
        int num1 = scanner.nextInt();
        System.out.print("Enter the second number: ");
        int num2 = scanner.nextInt();
        int sum = num1 + num2;
        System.out.println("The sum is: " + sum);
        scanner.close();
    }
}   

public class PrimeCheck {
    public static void main(String[] args) {
        int num = 29;
        boolean isPrime = true;
        if (num <= 1) {
            isPrime = false;
        } else {
            for (int i = 2; i <= num / 2; ++i) {
                if (num % i == 0) {
                    isPrime = false;
                    break;
                }
            }
        }
        if (isPrime)
            System.out.println(num + " is a prime number.");
        else
            System.out.println(num + " is not a prime number.");
    }
}   

You want to **run `SumCalculator` and `PrimeCheck` from the `Main` class**, instead of running each class separately.
Right now, each class has its own `main()` method — but you want **Main → run both → call logic**.

There are **two ways** to do this.

---

# ✅ **✔ BEST WAY: Move the logic into methods and call them from Main**

Modify each class so they don’t have their own `main()` — instead they expose methods.

---

## ✅ **Step 1: Update your classes**

### **Main.java**

```java
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        System.out.println("Hello, World!");

        SumCalculator sum = new SumCalculator();
        sum.calculateSum();

        PrimeCheck prime = new PrimeCheck();
        prime.checkPrime();
    }
}
```

---

### **SumCalculator.java**

```java
import java.util.Scanner;

class SumCalculator {

    void calculateSum() {
        Scanner scanner = new Scanner(System.in);
        System.out.print("Enter the first number: ");
        int num1 = scanner.nextInt();
        System.out.print("Enter the second number: ");
        int num2 = scanner.nextInt();
        int sum = num1 + num2;
        System.out.println("The sum is: " + sum);
        scanner.close();
    }
}
```

---

### **PrimeCheck.java**

```java
class PrimeCheck {

    void checkPrime() {
        int num = 29;
        boolean isPrime = true;

        if (num <= 1) {
            isPrime = false;
        } else {
            for (int i = 2; i <= num / 2; ++i) {
                if (num % i == 0) {
                    isPrime = false;
                    break;
                }
            }
        }

        if (isPrime)
            System.out.println(num + " is a prime number.");
        else
            System.out.println(num + " is not a prime number.");
    }
}
```

---

# 🟢 **Output when you run Main:**

```
Hello, World!
Enter the first number:
...
The sum is: ...
29 is a prime number.
```

---

# ✅ **✔ SECOND WAY (NOT recommended but allowed): Call other main() methods**

If you want to keep your code *exactly the same* but still run everything from Main:

### **Main.java**

```java
public class Main {
    public static void main(String[] args) {
        System.out.println("Hello, World!");

        // Calling other main methods
        SumCalculator.main(null);
        PrimeCheck.main(null);
    }
}
```

This works because `main()` is just a static method.

---

# 🔥 Which method should you use?

| Method                                              | When to Use                         |
| --------------------------------------------------- | ----------------------------------- |
| **Call methods like calculateSum() / checkPrime()** | Best practice, clean code, scalable |
| **Call SumCalculator.main()**                       | Quick testing, beginner demos       |

---


====================================================


Below is the **fully corrected version** of your program where:

✔ All your logic classes are kept
✔ Only **one** class has `public static void main()` → the **MainRunner**
✔ Each class exposes a **method**, not its own `main()`
✔ Main calls all of them → **single-entry execution**

This is the exact structure you need.

---

# ✅ **Final Clean Version — All classes runnable from ONE main class**

```java
import java.util.*;
import java.util.function.Function;
import java.util.stream.Collectors;
import java.util.Comparator;
import java.util.Arrays;

// ========================================
// MASTER MAIN CLASS
// ========================================
public class Main {
    public static void main(String[] args) {
        System.out.println("Running all programs from ONE main class\n");

        new SecondHighestWithStreams().run();
        new CharFrequency().run();
        new FirstNonRepeatedChar().run();
        new CommonElements().run();
        new RemoveDuplicates().run();
        new LongestWord().run();
    }
}

// ========================================
// 1. Second Highest Number
// ========================================
class SecondHighestWithStreams {
    void run() {
        int[] arr = {10, 45, 67, 23, 45, 88, 99};
        int secondHighest = Arrays.stream(arr)
                .boxed()
                .distinct()
                .sorted(Comparator.reverseOrder())
                .skip(1)
                .findFirst()
                .orElseThrow(() -> new RuntimeException("No second highest number"));
        System.out.println("Second Highest Number: " + secondHighest);
    }
}

// ========================================
// 2. Character Frequency
// ========================================
class CharFrequency {
    void run() {
        String input = "success";
        Map<Character, Long> frequency = input.chars()
                .mapToObj(c -> (char) c)
                .collect(Collectors.groupingBy(Function.identity(), Collectors.counting()));
        System.out.println("Char Frequency: " + frequency);
    }
}

// ========================================
// 3. First Non-Repeated Character
// ========================================
class FirstNonRepeatedChar {
    void run() {
        String str = "swiss";
        for (int i = 0; i < str.length(); i++) {
            char c = str.charAt(i);
            if (str.indexOf(c) == str.lastIndexOf(c)) {
                System.out.println("First Non-Repeated Character: " + c);
                return;
            }
        }
    }
}

// ========================================
// 4. Common Elements Between Two Arrays
// ========================================
class CommonElements {
    void run() {
        Integer[] arr1 = {1, 2, 3, 4, 5};
        Integer[] arr2 = {3, 4, 5, 6, 7};

        Set<Integer> set1 = Arrays.stream(arr1).collect(Collectors.toSet());
        Set<Integer> common = Arrays.stream(arr2)
                .filter(set1::contains)
                .collect(Collectors.toSet());

        System.out.println("Common Elements: " + common);
    }
}

// ========================================
// 5. Remove Duplicates
// ========================================
class RemoveDuplicates {
    void run() {
        int[] arr = {1, 2, 3, 2, 4, 3, 5};
        int[] unique = Arrays.stream(arr)
                .distinct()
                .toArray();
        System.out.println("Array Without Duplicates: " + Arrays.toString(unique));
    }
}

// ========================================
// 6. Longest Word in a Sentence
// ========================================
class LongestWord {
    void run() {
        String sentence = "Java makes coding elegant and fun";
        String longest = Arrays.stream(sentence.split(" "))
                .max(Comparator.comparingInt(String::length))
                .orElse("");
        System.out.println("Longest Word: " + longest);
    }
}
```

---

# ✅ **How it works**

### ✔ Only `MainRunner` has a `main()`

### ✔ Each class exposes a `run()` method

### ✔ `MainRunner` creates object instances and calls them:

```java
new SecondHighestWithStreams().run();
new CharFrequency().run();
...
```

This executes **all your programs sequentially** from one single main.

---


====================================================


Below is a **clean, enterprise-style Java project structure** with:

✔ **OOP design** (separation of concerns, classes grouped into services)
✔ **Proper folder structure**: `src/main/java/...`
✔ A **MainRunner** that runs all services
✔ Each program converted into a **Service class** with a clear responsibility

This resembles how real Java/Spring projects structure their code.

---

# 📁 **Final Java Project Structure**

```
your-project/
└── src/
    └── main/
        └── java/
            └── com/
                └── example/
                    ├── MainRunner.java
                    ├── service/
                    │   ├── SecondHighestService.java
                    │   ├── CharFrequencyService.java
                    │   ├── FirstNonRepeatedService.java
                    │   ├── CommonElementsService.java
                    │   ├── RemoveDuplicatesService.java
                    │   └── LongestWordService.java
                    └── model/
                        └── (optional future classes)
```

---

# ✅ **1. MainRunner (single entry point)**

`src/main/java/com/example/MainRunner.java`

```java
package com.example;

import com.example.service.*;

public class MainRunner {
    public static void main(String[] args) {

        System.out.println("Running All Algorithms using OOP + Project Structure\n");

        new SecondHighestService().run();
        new CharFrequencyService().run();
        new FirstNonRepeatedService().run();
        new CommonElementsService().run();
        new RemoveDuplicatesService().run();
        new LongestWordService().run();
    }
}
```

---

# ✅ **2. Services Folder (each class = one responsibility)**

---

## **Second Highest Number**

`src/main/java/com/example/service/SecondHighestService.java`

```java
package com.example.service;

import java.util.Arrays;
import java.util.Comparator;

public class SecondHighestService {

    public void run() {
        int[] arr = {10, 45, 67, 23, 45, 88, 99};
        int secondHighest = Arrays.stream(arr)
                .boxed()
                .distinct()
                .sorted(Comparator.reverseOrder())
                .skip(1)
                .findFirst()
                .orElseThrow(() -> new RuntimeException("No second highest number"));

        System.out.println("Second Highest Number: " + secondHighest);
    }
}
```

---

## **Character Frequency**

`src/main/java/com/example/service/CharFrequencyService.java`

```java
package com.example.service;

import java.util.Map;
import java.util.function.Function;
import java.util.stream.Collectors;

public class CharFrequencyService {

    public void run() {
        String input = "success";

        Map<Character, Long> frequency = input.chars()
                .mapToObj(c -> (char) c)
                .collect(Collectors.groupingBy(Function.identity(), Collectors.counting()));

        System.out.println("Character Frequency: " + frequency);
    }
}
```

---

## **First Non-Repeated Character**

`src/main/java/com/example/service/FirstNonRepeatedService.java`

```java
package com.example.service;

public class FirstNonRepeatedService {

    public void run() {
        String str = "swiss";

        for (int i = 0; i < str.length(); i++) {
            char c = str.charAt(i);
            if (str.indexOf(c) == str.lastIndexOf(c)) {
                System.out.println("First Non-Repeated Character: " + c);
                return;
            }
        }
    }
}
```

---

## **Common Elements**

`src/main/java/com/example/service/CommonElementsService.java`

```java
package com.example.service;

import java.util.Arrays;
import java.util.Set;
import java.util.stream.Collectors;

public class CommonElementsService {

    public void run() {
        Integer[] arr1 = {1, 2, 3, 4, 5};
        Integer[] arr2 = {3, 4, 5, 6, 7};

        Set<Integer> set1 = Arrays.stream(arr1).collect(Collectors.toSet());

        Set<Integer> common = Arrays.stream(arr2)
                .filter(set1::contains)
                .collect(Collectors.toSet());

        System.out.println("Common Elements: " + common);
    }
}
```

---

## **Remove Duplicates**

`src/main/java/com/example/service/RemoveDuplicatesService.java`

```java
package com.example.service;

import java.util.Arrays;

public class RemoveDuplicatesService {

    public void run() {
        int[] arr = {1, 2, 3, 2, 4, 3, 5};

        int[] unique = Arrays.stream(arr)
                .distinct()
                .toArray();

        System.out.println("Unique Elements: " + Arrays.toString(unique));
    }
}
```

---

## **Longest Word**

`src/main/java/com/example/service/LongestWordService.java`

```java
package com.example.service;

import java.util.Arrays;
import java.util.Comparator;

public class LongestWordService {

    public void run() {
        String sentence = "Java makes coding elegant and fun";

        String longest = Arrays.stream(sentence.split(" "))
                .max(Comparator.comparingInt(String::length))
                .orElse("");

        System.out.println("Longest Word: " + longest);
    }
}
```

---

# 🔥 **What you now have**

✔ A clean **OOP design**
✔ Each algorithm is in a dedicated **service class**
✔ One main entry point (`MainRunner`)
✔ Real Java project structure, just like enterprise projects
✔ Ready to run in IntelliJ, Eclipse, VS Code, or Maven/Gradle

---


==================================================


Great — now we’ll refactor your project into a **clean OOP architecture** using:

✔ **Interfaces**
✔ **Dependency Injection (DI)**
✔ **Loose coupling**
✔ **Single Responsibility Principle**
✔ Enterprise-style structure (close to Spring but without needing Spring yet)

This is how real Java backend projects are designed before adding frameworks.

---

# ✅ **Final Architecture (Using Interfaces + DI)**

```
com.example
│
├── MainRunner.java
│
├── di/
│   └── ServiceInjector.java        (Simple DI container)
│
└── service/
    ├── AlgorithmService.java      (Interface)
    │
    ├── impl/
    │   ├── SecondHighestService.java
    │   ├── CharFrequencyService.java
    │   ├── FirstNonRepeatedService.java
    │   ├── CommonElementsService.java
    │   ├── RemoveDuplicatesService.java
    │   └── LongestWordService.java
```

---

# ⭐ STEP 1 — Create a **Common Interface**

### `AlgorithmService.java`

```java
package com.example.service;

public interface AlgorithmService {
    void run();
}
```

Every algorithm class will now implement this interface.

---

# ⭐ STEP 2 — Implement Each Service (Loose Coupling)

---

## **SecondHighestService.java**

```java
package com.example.service.impl;

import com.example.service.AlgorithmService;
import java.util.Arrays;
import java.util.Comparator;

public class SecondHighestService implements AlgorithmService {

    @Override
    public void run() {
        int[] arr = {10, 45, 67, 23, 45, 88, 99};

        int secondHighest = Arrays.stream(arr)
                .boxed()
                .distinct()
                .sorted(Comparator.reverseOrder())
                .skip(1)
                .findFirst()
                .orElseThrow(() -> new RuntimeException("No second highest number"));

        System.out.println("Second Highest Number: " + secondHighest);
    }
}
```

---

## **CharFrequencyService.java**

```java
package com.example.service.impl;

import com.example.service.AlgorithmService;
import java.util.Map;
import java.util.function.Function;
import java.util.stream.Collectors;

public class CharFrequencyService implements AlgorithmService {

    @Override
    public void run() {
        String input = "success";

        Map<Character, Long> frequency = input.chars()
                .mapToObj(c -> (char) c)
                .collect(Collectors.groupingBy(Function.identity(), Collectors.counting()));

        System.out.println("Character Frequency: " + frequency);
    }
}
```

---

## **FirstNonRepeatedService.java**

```java
package com.example.service.impl;

import com.example.service.AlgorithmService;

public class FirstNonRepeatedService implements AlgorithmService {

    @Override
    public void run() {
        String str = "swiss";

        for (int i = 0; i < str.length(); i++) {
            char c = str.charAt(i);
            if (str.indexOf(c) == str.lastIndexOf(c)) {
                System.out.println("First Non-Repeated Character: " + c);
                return;
            }
        }
    }
}
```

---

## **CommonElementsService.java**

```java
package com.example.service.impl;

import com.example.service.AlgorithmService;
import java.util.Arrays;
import java.util.Set;
import java.util.stream.Collectors;

public class CommonElementsService implements AlgorithmService {

    @Override
    public void run() {
        Integer[] arr1 = {1, 2, 3, 4, 5};
        Integer[] arr2 = {3, 4, 5, 6, 7};

        Set<Integer> set1 = Arrays.stream(arr1).collect(Collectors.toSet());

        Set<Integer> common = Arrays.stream(arr2)
                .filter(set1::contains)
                .collect(Collectors.toSet());

        System.out.println("Common Elements: " + common);
    }
}
```

---

## **RemoveDuplicatesService.java**

```java
package com.example.service.impl;

import com.example.service.AlgorithmService;
import java.util.Arrays;

public class RemoveDuplicatesService implements AlgorithmService {

    @Override
    public void run() {
        int[] arr = {1, 2, 3, 2, 4, 3, 5};

        int[] unique = Arrays.stream(arr)
                .distinct()
                .toArray();

        System.out.println("Unique Elements: " + Arrays.toString(unique));
    }
}
```

---

## **LongestWordService.java**

```java
package com.example.service.impl;

import com.example.service.AlgorithmService;
import java.util.Arrays;
import java.util.Comparator;

public class LongestWordService implements AlgorithmService {

    @Override
    public void run() {
        String sentence = "Java makes coding elegant and fun";

        String longest = Arrays.stream(sentence.split(" "))
                .max(Comparator.comparingInt(String::length))
                .orElse("");

        System.out.println("Longest Word: " + longest);
    }
}
```

---

# ⭐ STEP 3 — Create a Simple DI Container

This behaves like a tiny version of **Spring DI**.

### `ServiceInjector.java`

```java
package com.example.di;

import com.example.service.AlgorithmService;
import com.example.service.impl.*;

import java.util.ArrayList;
import java.util.List;

public class ServiceInjector {

    public List<AlgorithmService> loadAllServices() {

        List<AlgorithmService> services = new ArrayList<>();

        services.add(new SecondHighestService());
        services.add(new CharFrequencyService());
        services.add(new FirstNonRepeatedService());
        services.add(new CommonElementsService());
        services.add(new RemoveDuplicatesService());
        services.add(new LongestWordService());

        return services;
    }
}
```

This would be replaced by Spring’s @ComponentScan later.

---

# ⭐ STEP 4 — MainRunner Uses DI to Run All Algorithms

### `MainRunner.java`

```java
package com.example;

import com.example.di.ServiceInjector;
import com.example.service.AlgorithmService;

public class MainRunner {

    public static void main(String[] args) {
        ServiceInjector injector = new ServiceInjector();

        // Load all algorithm services using DI
        for (AlgorithmService service : injector.loadAllServices()) {
            service.run();
        }
    }
}
```

---

# 🎉 **Now your project is:**

✔ Fully OOP
✔ Uses **interfaces** → loose coupling
✔ Uses **Dependency Injection** → no hard-wired logic
✔ Extensible (add new algorithm by implementing interface)
✔ Professional enterprise-like structure
✔ Ready to become a **Spring Boot project**
✔ Ready for **unit testing (Mockito / JUnit)**

---


========================================================


Nice — I’ll convert your algorithm services into a small Spring Boot REST API.
I’ll keep the same OOP + DI design, but now each algorithm is exposed as a REST endpoint that accepts JSON and returns JSON. You can run this as a standard Maven Spring Boot app (Java 17).

Below you’ll find:

* Project layout
* `pom.xml` (Maven)
* All Java source files (controller, services, models, application) ready to paste into `src/main/java`
* `application.properties` (minimal)
* Example `curl` requests for each endpoint

---

# Project layout

```
your-project/
├─ pom.xml
└─ src/
   └─ main/
      ├─ java/
      │  └─ com/
      │     └─ example/
      │        └─ demo/
      │           ├─ DemoApplication.java
      │           ├─ controller/
      │           │  └─ AlgorithmsController.java
      │           ├─ service/
      │           │  ├─ AlgorithmService.java
      │           │  ├─ impl/
      │           │  │  ├─ SecondHighestService.java
      │           │  │  ├─ CharFrequencyService.java
      │           │  │  ├─ FirstNonRepeatedService.java
      │           │  │  ├─ CommonElementsService.java
      │           │  │  ├─ RemoveDuplicatesService.java
      │           │  │  └─ LongestWordService.java
      │           └─ dto/
      │              ├─ IntArrayRequest.java
      │              ├─ TwoIntArrayRequest.java
      │              └─ StringRequest.java
      └─ resources/
         └─ application.properties
```

---

# `pom.xml`

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
                             http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.example</groupId>
    <artifactId>demo-algorithms</artifactId>
    <version>1.0.0</version>
    <packaging>jar</packaging>

    <name>demo-algorithms</name>

    <properties>
        <java.version>17</java.version>
        <spring.boot.version>3.1.4</spring.boot.version>
    </properties>

    <dependencyManagement>
        <dependencies>
            <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-dependencies</artifactId>
                <version>${spring.boot.version}</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
        </dependencies>
    </dependencyManagement>

    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

        <!-- Optional: for testing -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
                <version>${spring.boot.version}</version>
            </plugin>
        </plugins>
    </build>
</project>
```

---

# `application.properties`

`src/main/resources/application.properties`

```properties
server.port=8080
spring.main.allow-bean-definition-overriding=true
```

---

# Java sources

Create the following files under `src/main/java/com/example/demo/...`

### `DemoApplication.java`

```java
package com.example.demo;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class DemoApplication {
    public static void main(String[] args) {
        SpringApplication.run(DemoApplication.class, args);
    }
}
```

---

### DTOs

`dto/IntArrayRequest.java`

```java
package com.example.demo.dto;

public class IntArrayRequest {
    private int[] arr;

    public int[] getArr() { return arr; }
    public void setArr(int[] arr) { this.arr = arr; }
}
```

`dto/TwoIntArrayRequest.java`

```java
package com.example.demo.dto;

public class TwoIntArrayRequest {
    private int[] arr1;
    private int[] arr2;

    public int[] getArr1() { return arr1; }
    public void setArr1(int[] arr1) { this.arr1 = arr1; }

    public int[] getArr2() { return arr2; }
    public void setArr2(int[] arr2) { this.arr2 = arr2; }
}
```

`dto/StringRequest.java`

```java
package com.example.demo.dto;

public class StringRequest {
    private String input;

    public String getInput() { return input; }
    public void setInput(String input) { this.input = input; }
}
```

---

### Interface

`service/AlgorithmService.java`

```java
package com.example.demo.service;

public interface AlgorithmService {
    // marker interface for now; each implementation provides run-like behavior
}
```

(We use specific services via their concrete types in controller for clarity.)

---

### Services implementations

`service/impl/SecondHighestService.java`

```java
package com.example.demo.service.impl;

import org.springframework.stereotype.Service;
import java.util.Arrays;
import java.util.Comparator;

@Service
public class SecondHighestService {

    public int computeSecondHighest(int[] arr) {
        return Arrays.stream(arr)
                .boxed()
                .distinct()
                .sorted(Comparator.reverseOrder())
                .skip(1)
                .findFirst()
                .orElseThrow(() -> new IllegalArgumentException("No second highest number"));
    }
}
```

`service/impl/CharFrequencyService.java`

```java
package com.example.demo.service.impl;

import org.springframework.stereotype.Service;
import java.util.Map;
import java.util.function.Function;
import java.util.stream.Collectors;

@Service
public class CharFrequencyService {

    public Map<Character, Long> frequency(String input) {
        if (input == null) throw new IllegalArgumentException("input required");
        return input.chars()
                .mapToObj(c -> (char) c)
                .collect(Collectors.groupingBy(Function.identity(), Collectors.counting()));
    }
}
```

`service/impl/FirstNonRepeatedService.java`

```java
package com.example.demo.service.impl;

import org.springframework.stereotype.Service;

@Service
public class FirstNonRepeatedService {

    public Character firstNonRepeated(String s) {
        if (s == null) return null;
        for (int i = 0; i < s.length(); i++) {
            char c = s.charAt(i);
            if (s.indexOf(c) == s.lastIndexOf(c)) {
                return c;
            }
        }
        return null;
    }
}
```

`service/impl/CommonElementsService.java`

```java
package com.example.demo.service.impl;

import org.springframework.stereotype.Service;

import java.util.Arrays;
import java.util.Set;
import java.util.stream.Collectors;

@Service
public class CommonElementsService {

    public Set<Integer> common(int[] arr1, int[] arr2) {
        Set<Integer> set1 = Arrays.stream(arr1).boxed().collect(Collectors.toSet());
        return Arrays.stream(arr2).boxed()
                .filter(set1::contains)
                .collect(Collectors.toSet());
    }
}
```

`service/impl/RemoveDuplicatesService.java`

```java
package com.example.demo.service.impl;

import org.springframework.stereotype.Service;
import java.util.Arrays;

@Service
public class RemoveDuplicatesService {

    public int[] removeDuplicates(int[] arr) {
        return Arrays.stream(arr).distinct().toArray();
    }
}
```

`service/impl/LongestWordService.java`

```java
package com.example.demo.service.impl;

import org.springframework.stereotype.Service;

import java.util.Arrays;
import java.util.Comparator;

@Service
public class LongestWordService {

    public String findLongest(String sentence) {
        if (sentence == null || sentence.isBlank()) return "";
        return Arrays.stream(sentence.split("\\s+"))
                .max(Comparator.comparingInt(String::length))
                .orElse("");
    }
}
```

---

### REST Controller

`controller/AlgorithmsController.java`

```java
package com.example.demo.controller;

import com.example.demo.dto.IntArrayRequest;
import com.example.demo.dto.StringRequest;
import com.example.demo.dto.TwoIntArrayRequest;
import com.example.demo.service.impl.*;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.*;

import java.util.Map;
import java.util.Set;

@RestController
@RequestMapping("/api")
public class AlgorithmsController {

    private final SecondHighestService secondHighestService;
    private final CharFrequencyService charFrequencyService;
    private final FirstNonRepeatedService firstNonRepeatedService;
    private final CommonElementsService commonElementsService;
    private final RemoveDuplicatesService removeDuplicatesService;
    private final LongestWordService longestWordService;

    public AlgorithmsController(SecondHighestService secondHighestService,
                                CharFrequencyService charFrequencyService,
                                FirstNonRepeatedService firstNonRepeatedService,
                                CommonElementsService commonElementsService,
                                RemoveDuplicatesService removeDuplicatesService,
                                LongestWordService longestWordService) {
        this.secondHighestService = secondHighestService;
        this.charFrequencyService = charFrequencyService;
        this.firstNonRepeatedService = firstNonRepeatedService;
        this.commonElementsService = commonElementsService;
        this.removeDuplicatesService = removeDuplicatesService;
        this.longestWordService = longestWordService;
    }

    // 1) second highest
    @PostMapping("/second-highest")
    public ResponseEntity<?> secondHighest(@RequestBody IntArrayRequest req) {
        int[] arr = req.getArr();
        if (arr == null || arr.length < 2) {
            return ResponseEntity.badRequest().body(Map.of("error", "provide at least two numbers"));
        }
        int result = secondHighestService.computeSecondHighest(arr);
        return ResponseEntity.ok(Map.of("secondHighest", result));
    }

    // 2) char frequency
    @PostMapping("/char-frequency")
    public ResponseEntity<?> charFrequency(@RequestBody StringRequest req) {
        Map<Character, Long> freq = charFrequencyService.frequency(req.getInput());
        return ResponseEntity.ok(freq);
    }

    // 3) first non repeated char
    @PostMapping("/first-non-repeated")
    public ResponseEntity<?> firstNonRepeated(@RequestBody StringRequest req) {
        Character c = firstNonRepeatedService.firstNonRepeated(req.getInput());
        if (c == null) return ResponseEntity.ok(Map.of("firstNonRepeated", null));
        return ResponseEntity.ok(Map.of("firstNonRepeated", c.toString()));
    }

    // 4) common elements
    @PostMapping("/common-elements")
    public ResponseEntity<?> commonElements(@RequestBody TwoIntArrayRequest req) {
        int[] a1 = req.getArr1();
        int[] a2 = req.getArr2();
        if (a1 == null || a2 == null) return ResponseEntity.badRequest().body(Map.of("error", "provide both arrays"));
        Set<Integer> common = commonElementsService.common(a1, a2);
        return ResponseEntity.ok(Map.of("commonElements", common));
    }

    // 5) remove duplicates
    @PostMapping("/remove-duplicates")
    public ResponseEntity<?> removeDuplicates(@RequestBody IntArrayRequest req) {
        int[] input = req.getArr();
        if (input == null) return ResponseEntity.badRequest().body(Map.of("error", "provide array"));
        int[] unique = removeDuplicatesService.removeDuplicates(input);
        return ResponseEntity.ok(Map.of("unique", unique));
    }

    // 6) longest word
    @PostMapping("/longest-word")
    public ResponseEntity<?> longestWord(@RequestBody StringRequest req) {
        String longest = longestWordService.findLongest(req.getInput());
        return ResponseEntity.ok(Map.of("longestWord", longest));
    }
}
```

---

# How to run

1. Save files in the structure above.
2. Build and run:

```bash
mvn spring-boot:run
# or
mvn clean package
java -jar target/demo-algorithms-1.0.0.jar
```

Server will start on `http://localhost:8080`.

---

# Example `curl` requests

1. Second highest:

```bash
curl -X POST -H "Content-Type: application/json" \
  -d '{"arr":[10,45,67,23,45,88,99]}' \
  http://localhost:8080/api/second-highest
# Response: {"secondHighest":99?}  (actually highest 99, second 88)
```

2. Char frequency:

```bash
curl -X POST -H "Content-Type: application/json" \
  -d '{"input":"success"}' \
  http://localhost:8080/api/char-frequency
# Response: {"s":3,"u":1,"c":2,"e":1}  (ordering may vary)
```

3. First non-repeated char:

```bash
curl -X POST -H "Content-Type: application/json" \
  -d '{"input":"swiss"}' \
  http://localhost:8080/api/first-non-repeated
# Response: {"firstNonRepeated":"w"}
```

4. Common elements:

```bash
curl -X POST -H "Content-Type: application/json" \
  -d '{"arr1":[1,2,3,4,5], "arr2":[3,4,5,6,7]}' \
  http://localhost:8080/api/common-elements
# Response: {"commonElements":[3,4,5]} // order may vary
```

5. Remove duplicates:

```bash
curl -X POST -H "Content-Type: application/json" \
  -d '{"arr":[1,2,3,2,4,3,5]}' \
  http://localhost:8080/api/remove-duplicates
# Response: {"unique":[1,2,3,4,5]}
```

6. Longest word:

```bash
curl -X POST -H "Content-Type: application/json" \
  -d '{"input":"Java makes coding elegant and fun"}' \
  http://localhost:8080/api/longest-word
# Response: {"longestWord":"elegant"}
```

---


==============================================================


Nice — here’s a full, practical plan and ready-to-use artifacts to turn your single Spring Boot app into a **microservice architecture**. I’ll give:

1. an architecture overview and rationale
2. suggested service decomposition (what each microservice does)
3. project/module layout (Maven multi-module)
4. example source files (key classes) for the most important services (Algorithm Service + API Gateway + Auth)
5. Docker & Docker Compose for local dev
6. Kubernetes manifests (Deployment, Service, Ingress, HPA) for production-like deploy
7. Observability + tracing + messaging + config + discovery recommendations
8. CI/CD (GitHub Actions) pipeline sketch
9. Security, testing, and operational notes

Everything below is immediately actionable — copy/paste the code, create the modules, build images and run.

---

# 1) Architecture overview (high level)

Services:

* **api-gateway** — single entry point, routing + auth + rate-limit.
* **auth-service** — issues JWT tokens, user store (Postgres) and login/signup endpoints.
* **algorithm-service** — stateless microservice exposing the algorithms (second-highest, char-frequency, etc.). Scales horizontally.
* **user-service** *(optional)* — user profile persistence, quotas, audit.
* **config-server** — centralized config (Spring Cloud Config).
* **discovery-server** — service registry (Eureka / Consul).
* **message-broker** — Kafka or RabbitMQ for events (async processing, audit).
* **monitoring** — Prometheus + Grafana.
* **tracing** — OpenTelemetry / Zipkin.
* **logging** — ELK / Loki + Fluentd.

Design principles:

* Each service is a Spring Boot app (separate Maven module) with its own Dockerfile and image.
* Services are stateless (except auth/user which have DB).
* API Gateway handles auth, routing, and cross-cutting concerns (CORS, rate limits).
* Use JWT for auth between gateway and backend services.
* Use service discovery for dynamic routing in k8s or static config for cloud.

---

# 2) Service decomposition & responsibilities

1. **api-gateway**

   * Routes `/api/algorithms/**` → algorithm-service
   * Validates JWT for protected routes
   * Rate limiting, aggregation if needed

2. **auth-service**

   * `POST /auth/login` → returns JWT
   * `POST /auth/register` → create user
   * Stores users in Postgres

3. **algorithm-service**

   * `POST /algorithms/second-highest`
   * `POST /algorithms/char-frequency`
   * `POST /algorithms/first-non-repeated`
   * `POST /algorithms/common-elements`
   * `POST /algorithms/remove-duplicates`
   * `POST /algorithms/longest-word`
   * Exposes Prometheus metrics

4. **config-server**

   * Stores application.properties in a git repo; services pull config at startup

5. **discovery-server**

   * Eureka server for service registration (optional in k8s — k8s DNS can replace it)

---

# 3) Maven multi-module layout

```
root/
├─ pom.xml                              (parent)
├─ api-gateway/
│   └─ pom.xml
├─ auth-service/
│   └─ pom.xml
├─ algorithm-service/
│   └─ pom.xml
├─ config-server/
│   └─ pom.xml
├─ discovery-server/
│   └─ pom.xml
└─ infra/                               (k8s manifests, docker-compose)
```

Parent `pom.xml` should define dependencyManagement and modules.

---

# 4) Example code — algorithm-service (core)

`algorithm-service/src/main/java/com/example/algorithm/AlgorithmApplication.java`

```java
package com.example.algorithm;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class AlgorithmApplication {
    public static void main(String[] args) {
        SpringApplication.run(AlgorithmApplication.class, args);
    }
}
```

`algorithm-service/src/main/java/com/example/algorithm/controller/AlgorithmsController.java`

```java
package com.example.algorithm.controller;

import com.example.algorithm.dto.IntArrayRequest;
import com.example.algorithm.dto.StringRequest;
import com.example.algorithm.service.*;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.*;

import java.util.Map;
import java.util.Set;

@RestController
@RequestMapping("/algorithms")
public class AlgorithmsController {

    private final SecondHighestService secondHighestService;
    private final CharFrequencyService charFrequencyService;
    private final FirstNonRepeatedService firstNonRepeatedService;
    private final CommonElementsService commonElementsService;
    private final RemoveDuplicatesService removeDuplicatesService;
    private final LongestWordService longestWordService;

    public AlgorithmsController(SecondHighestService secondHighestService,
                                CharFrequencyService charFrequencyService,
                                FirstNonRepeatedService firstNonRepeatedService,
                                CommonElementsService commonElementsService,
                                RemoveDuplicatesService removeDuplicatesService,
                                LongestWordService longestWordService) {
        this.secondHighestService = secondHighestService;
        this.charFrequencyService = charFrequencyService;
        this.firstNonRepeatedService = firstNonRepeatedService;
        this.commonElementsService = commonElementsService;
        this.removeDuplicatesService = removeDuplicatesService;
        this.longestWordService = longestWordService;
    }

    @PostMapping("/second-highest")
    public ResponseEntity<?> secondHighest(@RequestBody IntArrayRequest req) {
        int val = secondHighestService.computeSecondHighest(req.getArr());
        return ResponseEntity.ok(Map.of("secondHighest", val));
    }

    @PostMapping("/char-frequency")
    public ResponseEntity<?> charFrequency(@RequestBody StringRequest req) {
        Map<Character, Long> res = charFrequencyService.frequency(req.getInput());
        return ResponseEntity.ok(res);
    }

    @PostMapping("/first-non-repeated")
    public ResponseEntity<?> firstNonRepeated(@RequestBody StringRequest req) {
        Character c = firstNonRepeatedService.firstNonRepeated(req.getInput());
        return ResponseEntity.ok(Map.of("firstNonRepeated", c));
    }

    @PostMapping("/common-elements")
    public ResponseEntity<?> commonElements(@RequestBody Map<String,int[]> req) {
        int[] a1 = req.get("arr1");
        int[] a2 = req.get("arr2");
        Set<Integer> common = commonElementsService.common(a1, a2);
        return ResponseEntity.ok(Map.of("common", common));
    }

    @PostMapping("/remove-duplicates")
    public ResponseEntity<?> removeDuplicates(@RequestBody IntArrayRequest req) {
        int[] unique = removeDuplicatesService.removeDuplicates(req.getArr());
        return ResponseEntity.ok(Map.of("unique", unique));
    }

    @PostMapping("/longest-word")
    public ResponseEntity<?> longestWord(@RequestBody StringRequest req) {
        String longest = longestWordService.findLongest(req.getInput());
        return ResponseEntity.ok(Map.of("longestWord", longest));
    }
}
```

Add service classes similar to the earlier Spring Boot service versions (they are stateless `@Service`-annotated beans). DTOs as before.

`algorithm-service/src/main/resources/application.yml` (excerpts)

```yaml
server:
  port: ${PORT:8081}
management:
  endpoints:
    web:
      exposure:
        include: "*"
spring:
  application:
    name: algorithm-service
```

Dockerfile:

```dockerfile
FROM eclipse-temurin:17-jre
ARG JAR_FILE=target/*.jar
COPY ${JAR_FILE} app.jar
ENTRYPOINT ["java","-jar","/app.jar"]
```

Expose Prometheus endpoint by including `micrometer-registry-prometheus` dependency.

---

# 5) Example code — api-gateway (Spring Cloud Gateway)

`api-gateway/src/main/java/com/example/gateway/GatewayApplication.java`

```java
package com.example.gateway;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class GatewayApplication {
   public static void main(String[] args) {
       SpringApplication.run(GatewayApplication.class, args);
   }
}
```

`application.yml`

```yaml
server:
  port: 8080
spring:
  cloud:
    gateway:
      routes:
        - id: algorithms
          uri: lb://ALGORITHM-SERVICE
          predicates:
            - Path=/algorithms/**
          filters:
            - StripPrefix=1
```

This configuration assumes service discovery (Eureka) or use static URIs ([http://algorithm-service:8081](http://algorithm-service:8081)) in k8s.

Use a pre-filter for JWT validation (or use Spring Security and OAuth2ResourceServer).

---

# 6) Example code — auth-service (JWT)

Implement `AuthController` with login that validates credentials and issues JWT signed with a secret or RSA key pair; store users in Postgres.

Key dependencies:

* `spring-boot-starter-data-jpa`
* `spring-boot-starter-security`
* `jjwt` or use Spring Security OAuth2 resource server.

---

# 7) Docker Compose (local dev)

`infra/docker-compose.yml`

```yaml
version: "3.8"
services:
  discovery:
    image: netflixoss/eureka # or local build
    environment: {}
    ports: ["8761:8761"]

  config-server:
    build: ../config-server
    ports: ["8888:8888"]

  algorithm-service:
    build: ../algorithm-service
    depends_on: [config-server, discovery]
    environment:
      - SPRING_PROFILES_ACTIVE=local
    ports:
      - "8081:8081"

  api-gateway:
    build: ../api-gateway
    depends_on: [algorithm-service, discovery]
    ports:
      - "8080:8080"

  auth-service:
    build: ../auth-service
    ports: ["8082:8082"]
    environment:
      - SPRING_DATASOURCE_URL=jdbc:postgresql://postgres:5432/authdb
    depends_on:
      - postgres

  postgres:
    image: postgres:15
    environment:
      POSTGRES_DB: authdb
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: postgres
    ports:
      - "5432:5432"

  prometheus:
    image: prom/prometheus
    ports: ["9090:9090"]
    volumes:
      - ./prometheus.yml:/etc/prometheus/prometheus.yml

  grafana:
    image: grafana/grafana
    ports: ["3000:3000"]
```

---

# 8) Kubernetes manifests (simplified)

Create `infra/k8s/` with manifests per service. Example for algorithm-service:

`algorithm-deployment.yaml`

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: algorithm-service
spec:
  replicas: 3
  selector:
    matchLabels:
      app: algorithm-service
  template:
    metadata:
      labels:
        app: algorithm-service
    spec:
      containers:
      - name: algorithm
        image: yourrepo/algorithm-service:1.0.0
        ports:
        - containerPort: 8081
        env:
        - name: SPRING_PROFILES_ACTIVE
          value: "k8s"
---
apiVersion: v1
kind: Service
metadata:
  name: algorithm-service
spec:
  selector:
    app: algorithm-service
  ports:
    - protocol: TCP
      port: 80
      targetPort: 8081
  type: ClusterIP
```

Ingress (NGINX):

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: demo-ingress
  annotations:
    kubernetes.io/ingress.class: nginx
spec:
  rules:
  - host: demo.local
    http:
      paths:
      - path: /algorithms
        pathType: Prefix
        backend:
          service:
            name: api-gateway
            port:
              number: 80
```

Add HPA:

```yaml
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: algorithm-hpa
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: algorithm-service
  minReplicas: 2
  maxReplicas: 10
  metrics:
    - type: Resource
      resource:
        name: cpu
        target:
          type: Utilization
          averageUtilization: 60
```

---

# 9) Observability & tracing

* Expose `/actuator/prometheus` (use micrometer) and scrape with Prometheus.
* Use OpenTelemetry collector for tracing + export to Jaeger/Zipkin.
* Use structured JSON logging (Logback) and ship logs to ELK / Loki.
* Add health checks (`/actuator/health`) and readiness probes in k8s.

k8s liveness/readiness example:

```yaml
livenessProbe:
  httpGet:
    path: /actuator/health/liveness
    port: 8081
  initialDelaySeconds: 30
  periodSeconds: 10
readinessProbe:
  httpGet:
    path: /actuator/health/readiness
    port: 8081
  initialDelaySeconds: 10
  periodSeconds: 5
```

---

# 10) Messaging and events

* Use Kafka or RabbitMQ for:

  * audit events (user requests)
  * async heavy compute (if any algorithm is heavy)
  * metrics or notifications

Publish a compact event on request start/complete for observability and billing.

---

# 11) Security: JWT + scopes

* Auth-service issues signed JWTs (RS256 recommended).
* API gateway validates JWT signature and passes user info through `X-User` headers to backend services.
* Backend services verify JWT signature or trust gateway (less secure).
* Enforce rate limits per API key / user.

---

# 12) CI/CD (GitHub Actions) pipeline skeleton

`.github/workflows/ci.yml`

```yaml
name: CI

on: [push]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Set up JDK 17
        uses: actions/setup-java@v4
        with:
          java-version: '17'
      - name: Build with Maven
        run: mvn -B -DskipTests clean package
      - name: Build Docker images
        run: |
          docker build -t yourrepo/algorithm-service:latest ./algorithm-service
          docker push yourrepo/algorithm-service:latest
# add similar steps for other services
```

Delivery: use ArgoCD or Flux for GitOps or GitHub Actions to kubectl apply manifests to cluster.

---

# 13) Testing & quality

* Unit tests (JUnit5) for each service; integration tests with Testcontainers for Postgres.
* Contract tests (Pact) between gateway and services.
* Use SonarCloud for code quality scanning.

---

# 14) Example: How to call the microservice architecture locally

1. Start infra with `docker-compose up --build` (discovery, config, prometheus, postgres)
2. Build and run services: `mvn -pl algorithm-service spring-boot:run` etc.
3. Obtain JWT from `auth-service` then call `POST http://localhost:8080/algorithms/second-highest` with JSON and Authorization header `Bearer <token>`

---






