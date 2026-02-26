# Notes on Java SpringBoot

## What is SpringBoot

- It is the autopilot of the Spring framework.
- It solves the configuration hell of Spring development.

## Core

- There are a lot of defaults for building.
- Automatically configures dependencies and drivers.
- Servers are embedded by default.
- Actuator. Built-in tools to monitor app health, metrics automatically.

## Pros

- Good for rapid development.
- Minimal setup.
- Reduced boilerplate.

## Things to learn from Spring

- Dependency Injection.
- Inversion of Control.
- Spring Annotations.
- The Bean Lifecycle.

## Spring MVC

- A specific module withing the Spring Framework designed for building web apps.
- Model: Represents data.
- View: The UI (mostly json).
- Controller: handles user requests.

How SpringBoot handles user requests: Request received -> Dispatcher takes it ->
Router is called -> Router sends to appropraite controller -> Response is sent.

## Beans

- Bean is an object that is intantiated, assembled, and managed by the Spring
Inversion of Control container.
- In normal Java, you create an object using `new MyClass()`. In Spring, you tell
it about your class and it creates the object for you. That managed object is 
called a bean.
- Beans are created using annotations.

### The `@Component` Family

Used on top of a class to make it a bean.

- `@Service`: for business logic.
- `@Repository`: for database access.
- `@Controller`: for web routing.

### The `@Bean` Annotation

Used inside a configuration class whne you are using a library you didn't write
so you cannot add the `@Component` annotations to the source code.

### Bean Scopes

By default, Spring Beans are **Singletons** (only one instance for the entire
application lifecycle). Anyone who asks for that bean gets the exact same object.

Other scopes include:

- Prototype: A new instance every time it is requested.
- Request/Session: Used in web apps (one bean per request).

### Usefulness of Beans

Imagine you have a `DatabaseConnection` class. You don't want to manually create
a new connection in every single file of your app. Instead, you make it a Bean.
Spring creates it once, handles the configuration, and "injects" it wherever 
it's needed. If you ever need to change the database params, you change it in 
one place (the Bean configuration), and the entire app is updated.

Example usage of beans:

1. The "Dependency"

```java
@Service
public class EmailService {
    public void send(String message) {
        System.out.println("Sending email: " + message);
    }
}
```

2. The "Injection"

```java
@RestController
public class NotificationController {

    private final EmailService emailService;

    // This is Constructor Injection
    // Spring sees this and says: "I have an EmailService bean in my box, 
    // I'll plug it in here!"
    public NotificationController(EmailService emailService) {
        this.emailService = emailService;
    }

    @GetMapping("/notify")
    public String triggerNotification() {
        emailService.send("Hello from Spring Boot!");
        return "Notification sent!";
    }
}
```

- There is loose coupling: the `NotificationController` doesn't care how
`EmailService` is created, it just knows it has a tool that can send emails.
Also, I can change the inner workings of `EmailService` without caring about 
who is using it.
