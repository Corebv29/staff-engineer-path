## Spring Chapter 1

Initial questions

1. What is inversion of control ? 
2. What is dependency injection ?
3. What is Jakarta ?

**Inversion of control** 
    Delegate the creation of dependencies to an external agent (Spring's container) *Initial thinking*

**Dependency injection**
Externalizes the creation and management of component dependencies
- Composition : Object A create Object B
- Aggregation : Object A itself retrieves Object B that already exists
- Dependency Injection : External party provide Object B to Object A

**Inversion of control**
    Design principle in which generic reusable components are used to control the execution of problem specific code, as in retrieving dependencies

The use of interfaces allows Spring to use JDK Dynamic Proxies to provide thinks like AOP 

**What are JDK Dynamic Proxies ?**
JDK Dynamic Proxies are a feature in Java that allows you to create proxy instances of interfaces at runtime. These proxies can intercept method calls and route them through a custom invocation handler, enabling behaviors like logging, access control, transaction management, or method call redirection without modifying the actual implementation.

**Key Concepts**

- Proxy: A class that implements one or more interfaces and delegates method calls to an InvocationHandler.

- InvocationHandler: An interface with a single invoke(Object proxy, Method method, Object[] args) method where the logic for handling method calls resides.

- Proxy.newProxyInstance(...): A factory method used to create a dynamic proxy instance.

```java
import java.lang.reflect.*;

interface Service {
    void perform();
}

class ServiceImpl implements Service {
    public void perform() {
        System.out.println("Executing service");
    }
}

class LoggingHandler implements InvocationHandler {
    private final Object target;

    public LoggingHandler(Object target) {
        this.target = target;
    }

    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
        System.out.println("Before method: " + method.getName());
        Object result = method.invoke(target, args);
        System.out.println("After method: " + method.getName());
        return result;
    }
}

public class Main {
    public static void main(String[] args) {
        Service realService = new ServiceImpl();
        Service proxyInstance = (Service) Proxy.newProxyInstance(
            Service.class.getClassLoader(),
            new Class<?>[] { Service.class },
            new LoggingHandler(realService)
        );

        proxyInstance.perform();  // Logs before and after calling real method
    }
}
```
**Analogy**

Think of dynamic proxies like a "wrapper" around a method call — they don’t replace the method, they just intercept it, do something before or after, and optionally modify or suppress the actual call.

---

Spring acts like a container that provide instances of you classes with all the dependencies they need.

**What is JavaBeans naming convention ?**

| Element | Convention |
|--------|-------------|
| **Property (field)** | `private` with no special name rules (e.g., `name`, `age`) |
| **Getter** | `getX()` for non-boolean, `isX()` for boolean (e.g., `getName()`, `isActive()`) |
| **Setter** | `setX(value)` (e.g., `setName(String name)`) |
| **Method name format** | `get/set` + **capitalized** property name (e.g., `getFirstName` for `firstName`) |
| **No-arg constructor** | Class should have a public no-arg constructor |

### Example

```java
public class Person {
    private String name;
    private boolean active;

    public Person() {}  // No-arg constructor

    public String getName() { return name; }
    public void setName(String name) { this.name = name; }

    public boolean isActive() { return active; }
    public void setActive(boolean active) { this.active = active; }
}
```

🚀 Why It Matters

- Introspection: Java frameworks (like Spring, JavaFX, JSP) use reflection to auto-detect properties via getters/setters.

- Serialization/Deserialization: Libraries like Jackson or Gson rely on these conventions to convert Java objects to/from JSON or XML.

- UI Tools: Swing, JavaFX, and other GUI builders can bind UI elements to JavaBean properties.

⚠️ Gotchas

- getURL() is correct for a property named URL, not uRL

- Boolean fields must use isX() as getter, not getX()

- Capitalization after get/set/is follows JavaBeans specification, which slightly differs from standard camelCase in edge cases


### Java Reflection

Java Reflection is a feature in the Java programming language that allows a program to inspect and manipulate the runtime behavior of classes, interfaces, fields, and methods — even if they are private.

It’s part of the java.lang.reflect package and allows you to:

- Inspect class metadata (like class name, methods, fields, constructors).

- Instantiate new objects.

- Invoke methods or access fields dynamically (at runtime).

- Modify private members, bypassing normal access control.

#### Example

```java
import java.lang.reflect.Method;

public class Demo {
    private void secretMethod() {
        System.out.println("Secret method called!");
    }

    public static void main(String[] args) throws Exception {
        Demo obj = new Demo();
        Method method = Demo.class.getDeclaredMethod("secretMethod");
        method.setAccessible(true); // bypass private visibility
        method.invoke(obj); // call the method
    }
}
```
