
# AEM 🚀

## AEM Developer Cheat Sheet

⸻

### AEM Annotations

1. @Model

Use Case: Used to define a Sling Model, which adapts a resource or request to a Java object. It’s the primary annotation for working with Sling Models in AEM. You use this when you want to map a Java class to a resource or request to retrieve and manipulate data for use in the front-end (e.g., in HTL).

Example:

```java

import org.apache.sling.models.annotations.Model;
import org.apache.sling.models.annotations.injectorspecific.ValueMapValue;
import org.apache.sling.api.resource.Resource;

@Model(adaptables = Resource.class)
public class PageModel {

    @ValueMapValue
    private String title;

    @ValueMapValue
    private String description;

    public String getTitle() {
        return title != null ? title : "Default Title";
    }

    public String getDescription() {
        return description != null ? description : "Default Description";
    }
}

```

When to Use:
	•	When you need to adapt resources to a Java class.
	•	When working with HTL (Sling Models are used to pass data to HTL templates).

⸻


2. @ValueMapValue

Use Case: Injects values from the JCR into fields in the Sling Model. This annotation is used for fields that should get their values from a ValueMap of a resource, such as properties of a content node.

Example:

```java


import org.apache.sling.models.annotations.Model;
import org.apache.sling.models.annotations.injectorspecific.ValueMapValue;

@Model(adaptables = Resource.class)
public class AuthorModel {

    @ValueMapValue
    private String authorName;

    @ValueMapValue
    private String authorBio;

    public String getAuthorName() {
        return authorName != null ? authorName : "Unknown Author";
    }

    public String getAuthorBio() {
        return authorBio != null ? authorBio : "No bio available";
    }
 }
 
}

```

When to Use:
	•	When you need to inject properties of a resource directly into a field in your Sling Model.
	•	When working with properties of content nodes (e.g., page properties, metadata).

⸻

3. @Inject

Use Case: Injects AEM-specific services into a Sling Model. This can be used to inject various services (like SlingHttpServletRequest, ResourceResolver, etc.) into a model for easy access.

Example:

```java

import org.apache.sling.models.annotations.Model;
import org.apache.sling.api.resource.ResourceResolver;
import org.apache.sling.models.annotations.Inject;

@Model(adaptables = Resource.class)
public class SiteModel {

    @Inject
    private ResourceResolver resourceResolver;

    public String getSiteName() {
        return resourceResolver.getContextPath();
    }
}

```

When to Use:
	•	When you need to inject AEM-specific services into your Sling Models, such as ResourceResolver, SlingHttpServletRequest, or SlingHttpServletResponse.

⸻

4. @Self

Use Case: This annotation is used for injecting the current request or resource into a field of a Sling Model. It’s commonly used to inject the SlingHttpServletRequest or the current Resource into the model.

Example:


```java


import org.apache.sling.models.annotations.Model;
import org.apache.sling.api.servlets.SlingHttpServletRequest;
import org.apache.sling.models.annotations.injectorspecific.Self;

@Model(adaptables = SlingHttpServletRequest.class)
public class RequestModel {

    @Self
    private SlingHttpServletRequest request;

    public String getRequestPath() {
        return request.getRequestURI();
    }
}

```

When to Use:
	•	When you need to inject the current request (SlingHttpServletRequest) or resource (Resource) in a Sling Model to access request or resource-specific data.

⸻

5. @PostConstruct

Use Case: Marks a method that should be called after the model has been created and all fields have been injected. It’s used to perform initialization tasks that depend on the injected fields.

Example:

import org.apache.sling.models.annotations.Model;
import org.apache.sling.models.annotations.PostConstruct;

@Model(adaptables = Resource.class)
public class InitModel {

    private String initializedData;

    @PostConstruct
    public void init() {
        // Perform initialization after all fields are injected
        initializedData = "Initialization Complete";
    }

    public String getInitializedData() {
        return initializedData;
    }
}

When to Use:
	•	When you need to perform initialization tasks (e.g., setting default values, complex logic) after the model has been constructed and dependencies have been injected.

⸻

6. @DesignModel

Use Case: Used for binding a Sling Model to a specific component for the design dialog. It provides a model that is specifically tailored to handle the design dialog configurations in AEM.

Example:

```

import org.apache.sling.models.annotations.Model;
import org.apache.sling.models.annotations.DesignModel;
import org.apache.sling.api.resource.Resource;

@Model(adaptables = Resource.class)
@DesignModel
public class DesignDialogModel {

    private String componentTitle;

    public String getComponentTitle() {
        return componentTitle;
    }

    public void setComponentTitle(String title) {
        this.componentTitle = title;
    }
}


```

When to Use:
	•	When working with design dialogs in AEM, typically used in editable templates.
	•	When you need to create models that will provide the design-specific properties.

⸻

7. @SlingObject

Use Case: Used to inject Sling-specific objects like the SlingHttpServletRequest, SlingHttpServletResponse, or SlingContext into a Sling Model.



Example:

```java

import org.apache.sling.models.annotations.Model;
import org.apache.sling.api.servlets.SlingHttpServletRequest;
import org.apache.sling.models.annotations.injectorspecific.SlingObject;

@Model(adaptables = SlingHttpServletRequest.class)
public class RequestInfoModel {

    @SlingObject
    private SlingHttpServletRequest request;

    public String getRequestURI() {
        return request.getRequestURI();
    }
}

```

When to Use:
	•	When you need to access low-level Sling-specific objects such as SlingHttpServletRequest or SlingContext in your model.

⸻

8. @RequestAttribute

Use Case: Used to inject request attributes into a Sling Model. It allows you to access values passed via request attributes.


```java

Example:

import org.apache.sling.models.annotations.Model;
import org.apache.sling.models.annotations.injectorspecific.RequestAttribute;

@Model(adaptables = SlingHttpServletRequest.class)
public class RequestAttributeModel {

    @RequestAttribute(name = "userEmail")
    private String userEmail;

    public String getUserEmail() {
        return userEmail != null ? userEmail : "No Email Provided";
    }
}


```

When to Use:
	•	When you need to inject request attributes (e.g., values passed in the request scope) into a Sling Model.

⸻

9. @Source

Use Case: This annotation is used to inject values from a custom source, such as an external service, or a custom repository.

Example:

```java

import org.apache.sling.models.annotations.Model;
import org.apache.sling.models.annotations.injectorspecific.Source;

@Model(adaptables = Resource.class)
public class ExternalServiceModel {

    @Source("externalService")
    private ExternalService externalService;

    public String fetchExternalData() {
        return externalService.getData();
    }
}

```


When to Use:
	•	When you need to inject values or services from an external source like an external service or custom repository into a Sling Model.

⸻

10. @Exporter

Use Case: This annotation is used for exporting a Sling Model as a JSON object, which can be useful in headless CMS or API-based use cases.

Example:


```java

import org.apache.sling.models.annotations.Model;
import org.apache.sling.models.annotations.exporter.Exporters;
import org.apache.sling.models.annotations.exporter.JsonExporter;

@Model(adaptables = Resource.class)
@Exporters({ @Exporter(name = "jackson", extensions = "json") })
public class JsonExportModel {

    private String title;
    private String description;

    public String getTitle() {
        return title;
    }

    public String getDescription() {
        return description;
    }
}


```

When to Use:
	•	When building a headless AEM architecture where data is exposed as JSON through the Sling Model.

⸻

Conclusion

AEM provides a wide variety of annotations for simplifying and enhancing development, especially when dealing with Sling Models and components. These annotations can be used to inject dependencies, adapt resources, handle events, and simplify interactions with AEM’s rich set of services. Using them effectively can significantly reduce boilerplate code and improve maintainability in your AEM projects.



⸻

### AEM Java Core Components Patterns

1. Use of Sling Models

Sling Models are used to expose Java objects as resources in AEM. It allows you to map Java POJOs to Sling resources, simplifying the process of working with JCR nodes and properties.

Use Case:

A Sling Model is typically used to retrieve and manipulate data from JCR and make it available for rendering in a JSP, HTL, or any other front-end technology.

Code Snippet:

```java


import org.apache.sling.api.resource.Resource;
import org.apache.sling.models.annotations.Model;
import org.apache.sling.models.annotations.injectorspecific.ValueMapValue;

@Model(adaptables = Resource.class)
public class PageModel {

    @ValueMapValue
    private String title;

    @ValueMapValue
    private String description;

    public String getTitle() {
        return title != null ? title : "Default Title";
    }

    public String getDescription() {
        return description != null ? description : "Default Description";
    }
}

```

Explanation:

In this example, PageModel is a Sling Model that adapts a Resource to Java and injects the title and description from the JCR repository. The values are accessed via ValueMapValue annotations.

⸻

2. HTL (HTML Template Language) Integration

HTL (formerly known as Sightly) is used to separate logic from HTML in AEM. Java backends can pass the required data to HTL templates using models and other helper classes.

Use Case:

HTL is commonly used for rendering data on the front end, with Java classes providing the business logic and resource retrieval.

Code Snippet:

```java


import org.apache.sling.api.resource.Resource;
import org.apache.sling.models.annotations.Model;
import org.apache.sling.models.annotations.injectorspecific.ValueMapValue;

@Model(adaptables = Resource.class)
public class HeaderModel {

    @ValueMapValue
    private String headerTitle;

    public String getHeaderTitle() {
        return headerTitle;
    }
}

```


HTL Example:

```html

<div>
    <h1>${model.headerTitle}</h1>
</div>


```

Explanation:

The Java model HeaderModel retrieves the headerTitle from the JCR, and HTL uses this model to display the title in the frontend.

⸻

3. Servlets in AEM

Servlets are used to handle HTTP requests. You can use AEM’s SlingSafeMethodsServlet or SlingAllMethodsServlet to create a custom servlet that processes HTTP requests.

Use Case:

Custom servlets in AEM are typically used for business logic that needs to interact with the request and response, such as form submissions or RESTful endpoints.

Code Snippet:
```java

import org.apache.sling.api.servlets.SlingAllMethodsServlet;
import org.apache.sling.api.servlets.Servlet;
import org.osgi.service.component.annotations.Component;
import org.osgi.framework.Constants;

import javax.servlet.Servlet;

@Component(service = Servlet.class, 
           property = { 
               Constants.SERVICE_DESCRIPTION + "=Custom Servlet",
               "sling.servlet.methods=POST",
               "sling.servlet.paths=/bin/customservlet"
           })
public class CustomServlet extends SlingAllMethodsServlet {

    @Override
    protected void doPost(SlingHttpServletRequest request, SlingHttpServletResponse response) throws ServletException, IOException {
        String param = request.getParameter("param");
        response.getWriter().write("Received param: " + param);
    }
}


```

Explanation:

This servlet is registered to handle POST requests at the /bin/customservlet path. It retrieves a parameter from the request and writes a response.

⸻

4. Event Handling in AEM

AEM uses Sling Event Handlers to listen for various events, such as node changes, creating a robust mechanism for triggering actions.

Use Case:

Event handlers are often used for custom workflows like updating search indexes, triggering notifications, or executing background tasks when content is added or modified.

Code Snippet:

```java


import org.apache.sling.event.jobs.Job;
import org.apache.sling.event.jobs.JobConsumer;
import org.apache.sling.event.jobs.JobManager;
import org.osgi.service.component.annotations.Component;
import org.apache.sling.event.jobs.JobConsumerConfiguration;

@Component(service = JobConsumer.class)
public class CustomJobConsumer implements JobConsumer {

    @Override
    public JobConsumerResult processJob(Job job) {
        // Custom logic here
        System.out.println("Job processed: " + job.getId());
        return JobConsumerResult.OK;
    }
}

```

Explanation:

This code demonstrates how to create a custom JobConsumer that handles events. The processJob method is called when a job event occurs, and custom logic can be applied.

⸻

5. Sling Filters

Sling Filters allow you to modify the HTTP request and response before it reaches a servlet, providing a mechanism to preprocess and postprocess HTTP requests.

Use Case:

Filters are useful for security, logging, and modifying requests or responses based on certain conditions.

Code Snippet:

```java


import org.apache.sling.api.filter.Filter;
import org.apache.sling.api.filter.FilterChain;
import org.apache.sling.api.filter.FilterConfig;
import org.apache.sling.api.servlets.SlingSafeMethodsServlet;
import org.osgi.framework.Constants;
import org.osgi.service.component.annotations.Component;

import javax.servlet.Filter;
import javax.servlet.ServletException;
import java.io.IOException;

@Component(service = Filter.class)
public class CustomFilter implements Filter {

    @Override
    public void doFilter(SlingHttpServletRequest request, SlingHttpServletResponse response, FilterChain chain)
            throws IOException, ServletException {
        // Custom logic before servlet is invoked
        System.out.println("Filter applied");

        // Proceed to the next filter or servlet
        chain.doFilter(request, response);
    }

    @Override
    public void init(FilterConfig filterConfig) throws ServletException {
    }

    @Override
    public void destroy() {
    }
}


```

Explanation:

This filter applies custom logic before the request is passed to the servlet. It can be used for actions like logging, security checks, etc.

⸻

6. OSGi Services and Dependency Injection

In AEM, OSGi services are used to implement and provide reusable services. You can inject OSGi services into components via annotations or manually.

Use Case:

Services are useful for shared logic, like sending emails, interacting with third-party services, or handling content delivery logic.

Code Snippet:
```

import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;

@Component(service = MyComponent.class)
public class MyComponent {

    @Reference
    private EmailService emailService;

    public void sendEmail(String recipient, String subject, String message) {
        emailService.sendEmail(recipient, subject, message);
    }
}

```

Explanation:

This code demonstrates how to inject an EmailService OSGi service into a component using the @Reference annotation, which can then be used to send emails.

⸻

7. AEM Sling Jobs API

The Sling Jobs API provides a way to handle long-running background tasks in AEM. You can schedule, enqueue, and manage jobs in AEM.

Use Case:

This is useful for tasks like processing large data imports or background tasks that shouldn’t be handled in real-time.

Code Snippet:

```

import org.apache.sling.event.jobs.JobManager;
import org.apache.sling.event.jobs.Job;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;

@Component(service = JobService.class)
public class JobService {

    @Reference
    private JobManager jobManager;

    public void createJob() {
        Job job = jobManager.addJob("myJobTopic", null);
        job.addJobEventListener(event -> {
            System.out.println("Job event triggered: " + event.getJob().getId());
        });
    }
}
```


Explanation:

In this example, a new job is created and an event listener is added. The job will be processed asynchronously, and the listener will handle the results.

⸻

Conclusion

These patterns and practices in AEM Java development allow for the creation of modular, reusable, and scalable solutions within AEM. Leveraging Sling Models, servlets, event handling, filters, OSGi services, and job management leads to a robust and maintainable AEM platform.

⸻





## AEM System Architecture  


### AEM Development Fundamentals  
- **Component Development**  
- **Sling Models**  
- **JCR & Oak Repository**  
- **OSGi Services**  
- **Dispatcher Caching**  

###  AEM Best Practices  
- Code Reusability  
- Performance Optimization  
- Security & Permissions  

### Third-Party Integrations  
- **GraphQL API**  
- **Headless CMS Implementation**  

###  Key Interview Topics  
- **SSSR (Streaming Server-Side Rendering)**  
- **React & AEM Headless Integration**  
- **AEM as a Cloud Service vs. On-Prem**  

### Deployment Strategies  
- Blue-Green Deployment  
- Rolling Updates  
- Feature Flagging

  

## AEM Architect Interview Preparation

Preparing for an Adobe Experience Manager (AEM) Architect Role requires a deep understanding of AEM’s architecture, implementation strategies, and integration with other systems. Below is a detailed guide to help you answer potential interview questions effectively.

---

## 1. General AEM Architecture Questions

**Question:** Can you explain the AEM architecture?

### Answer Framework:
- Start by mentioning **Sling** (the web framework), **JCR** (Java Content Repository), and **OSGi** (modular development framework).
- Describe how AEM uses Sling's resource resolution mechanism to map request URLs to content nodes.
- Highlight **Granite**, AEM’s foundation layer, and its core services.
- Discuss the content repository structure (e.g., `/content`, `/apps`, `/libs`, etc.) and its importance in the architecture.

### Key Points to Emphasize:
- **Hierarchical storage** in CRX/JCR.
- **AEM’s RESTful design** with Sling Models.
- **Editable Templates, Components, and ClientLibs** for frontend development.

---

## 2. Workflow and Content Management

**Question:** How would you design and implement a custom workflow in AEM?

### Answer Framework:
- Identify the business use case (e.g., content approval, translation).
- Use **AEM’s Workflow Console** to configure workflows or create a custom one.
- Mention **Workflow Steps** (process, participant, or automated steps).
- Explain using **Java delegates or ECMA scripts** to customize steps.
- Ensure workflows are optimized to avoid repository performance issues.

### Key Points to Emphasize:
- Use **best practices** for custom workflows to avoid performance degradation.
- Monitor workflows using **JMX or AEM tools**.
- Demonstrate familiarity with **Granite Workflow APIs**.

---

## 3. Integration with Other Systems

**Question:** How do you integrate AEM with external systems like a PIM or CRM?

### Answer Framework:
- Describe AEM APIs (e.g., **Sling, JCR, Asset Manager**) and frameworks like **OSGi Services**.
- Mention **RESTful APIs** or **SOAP** for external system communication.
- Highlight **Synchronous/Asynchronous calls** using Event Handlers or Message Queues (e.g., **Apache Kafka, ActiveMQ**).
- Explain the use of **Adobe I/O Runtime** for serverless integrations.

### Key Points to Emphasize:
- How **Dispatcher caching** is managed during integration.
- Proper use of **service users** for secured API interactions.
- Example: Fetching product data from a **PIM system** to populate content fragments dynamically.

---

## More Topics Covered:
- Best Practices for AEM Component Development.
- AEM Performance Optimization Strategies.
- AEM Cloud and Headless CMS Approaches.

### 📌 **Tip:**
> Be prepared to discuss **real-world scenarios** and explain how AEM’s modular architecture can help solve enterprise-level challenges.

---

⸻

# Java

## Java Fundamentals in AEM React GraphQL Integration

1. Core Java Principles
	•	OOP (Object-Oriented Programming) → Encapsulation, Inheritance, Polymorphism, and Abstraction.
	•	Multi-threading & Concurrency → Essential for handling high-traffic e-commerce or banking applications.
	•	Java Streams & Functional Programming → Efficient data transformation and manipulation.
	•	Exception Handling → Ensures resilience against failures in API calls and data processing.

2. Spring Boot & OSGi Framework
	•	Spring Boot is commonly used in microservices for banking applications.
	•	OSGi (Open Service Gateway Initiative) is used within AEM for modular development.
	•	Dependency Injection for managing services and configurations.

⸻

## Architecture for E-commerce & Banking Applications

1. E-Commerce (AEM + React + GraphQL)
	•	Frontend: React with Apollo Client for GraphQL queries.
	•	Backend: AEM with GraphQL persisted queries for optimized content delivery.
	•	Search: Elasticsearch/Solr for product catalog searches.
	•	Payment & Order Processing: Spring Boot Microservices handling secure transactions.
	•	Caching: Dispatcher & CDN (Akamai, Cloudflare) for performance optimization.

2. Banking (AEM + React + GraphQL)
	•	Security: OAuth 2.0, JWT Authentication.
	•	Microservices: Spring Boot + Kafka for real-time transaction processing.
	•	Data Management: PostgreSQL, MongoDB for handling financial data.
	•	GraphQL Gateway: Unifying multiple backend services (accounts, loans, credit scores).



## Key Java Design Patterns Used

1. Singleton Pattern (For Global Configurations & Services)
	•	Used in AEM OSGi services to maintain a single instance of frequently accessed objects (e.g., GraphQL Client, API Connectors).
	•	Ensures that expensive objects (e.g., database connections, HTTP clients) are reused instead of recreated.

2. Factory Pattern (For Creating API Clients & Data Models)
	•	Used for dynamically creating service clients, such as GraphQL API clients, based on request parameters.
	•	Commonly applied in Headless AEM where different API endpoints might need different configurations.

3. Builder Pattern (For Creating Complex Queries & DTOs)
	•	Applied in GraphQL query construction to build dynamic and reusable queries efficiently.
	•	Commonly used for constructing E-commerce Product Catalog queries.

4. Repository Pattern (For Data Access & Persistence)
	•	Used in Banking Applications for abstracting database interactions.
	•	Helps separate database logic from business logic in AEM’s JCR (Java Content Repository).

5. Strategy Pattern (For Payment & Authentication Mechanisms)
	•	Used in E-commerce checkout flows where multiple payment gateways (PayPal, Stripe, etc.) need to be supported dynamically.
	•	In Banking apps, different authentication strategies (OAuth, JWT, SAML) can be implemented.

6. Observer Pattern (For Event-Driven Architecture)
	•	Used in real-time transaction notifications in banking and inventory updates in e-commerce.
	•	Helps in building event-driven AEM components.

7. Adapter Pattern (For External API Integrations)
	•	Used in GraphQL to REST API conversions.
	•	Helps in integrating legacy systems with modern microservices.

8. CQRS (Command Query Responsibility Segregation)
	•	Applied in GraphQL mutations vs queries.
	•	Helps separate read vs write concerns in high-traffic applications.

⸻
React GraphQL Integration in E-commerce and Banking web applications, Java fundamentals and design patterns play a crucial role in building a scalable, maintainable, and secure architecture. Here’s a breakdown:




Here are Java code snippets demonstrating key design patterns commonly used in AEM, e-commerce, and banking applications:

⸻

1. Singleton Pattern (Global Configurations & Services)

Used for maintaining a single instance of expensive objects like GraphQL clients.

```

public class GraphQLClient {
    private static GraphQLClient instance;
    
    private GraphQLClient() {
        // Private constructor to prevent instantiation
    }
    
    public static synchronized GraphQLClient getInstance() {
        if (instance == null) {
            instance = new GraphQLClient();
        }
        return instance;
    }
    
    public String executeQuery(String query) {
        // Logic to execute GraphQL query
        return "Query Result";
    }
}


```


⸻

2. Factory Pattern (Creating API Clients & Data Models)

Dynamically creates different API clients based on request parameters.

```

public interface APIClient {
    void fetchData();
}

public class GraphQLAPIClient implements APIClient {
    public void fetchData() {
        System.out.println("Fetching data from GraphQL API");
    }
}

public class RestAPIClient implements APIClient {
    public void fetchData() {
        System.out.println("Fetching data from REST API");
    }
}

public class APIClientFactory {
    public static APIClient getClient(String type) {
        if ("GraphQL".equalsIgnoreCase(type)) {
            return new GraphQLAPIClient();
        } else if ("REST".equalsIgnoreCase(type)) {
            return new RestAPIClient();
        }
        throw new IllegalArgumentException("Invalid API Client Type");
    }
}

```



⸻

3. Builder Pattern (Creating Complex Queries & DTOs)

Builds dynamic and reusable GraphQL queries.


```

public class GraphQLQueryBuilder {
    private String query;

    public GraphQLQueryBuilder() {
        this.query = "";
    }

    public GraphQLQueryBuilder select(String field) {
        this.query += field + " ";
        return this;
    }

    public GraphQLQueryBuilder from(String object) {
        this.query = object + " { " + this.query + "}";
        return this;
    }

    public String build() {
        return "query { " + this.query + " }";
    }

    public static void main(String[] args) {
        String query = new GraphQLQueryBuilder()
                            .select("id")
                            .select("name")
                            .from("product")
                            .build();
        System.out.println(query);
    }
}


```

⸻

4. Repository Pattern (Data Access & Persistence)

Separates database logic from business logic.

Interface

```

public interface ProductRepository {
    Product getProductById(int id);
    void saveProduct(Product product);
}

```
Class

```

public class JCRProductRepository implements ProductRepository {
    public Product getProductById(int id) {
        System.out.println("Fetching product from JCR with ID: " + id);
        return new Product(id, "Sample Product");
    }

    public void saveProduct(Product product) {
        System.out.println("Saving product in JCR: " + product.getName());
    }
}


```
⸻

5. Strategy Pattern (Payment & Authentication Mechanisms)

Dynamically selects different payment or authentication strategies.

Interface

```

public interface PaymentStrategy {
    void pay(double amount);
}



```
Class

```

`

public class PayPalPayment implements PaymentStrategy {
    public void pay(double amount) {
        System.out.println("Paid $" + amount + " using PayPal.");
    }
}

public class CreditCardPayment implements PaymentStrategy {
    public void pay(double amount) {
        System.out.println("Paid $" + amount + " using Credit Card.");
    }
}

public class PaymentContext {
    private PaymentStrategy strategy;

    public PaymentContext(PaymentStrategy strategy) {
        this.strategy = strategy;
    }

    public void executePayment(double amount) {
        strategy.pay(amount);
    }

    public static void main(String[] args) {
        PaymentContext context = new PaymentContext(new PayPalPayment());
        context.executePayment(100.0);
    }
}


```



⸻

6. Observer Pattern (Event-Driven Architecture)

Used for real-time notifications.

Interface
```

import java.util.ArrayList;
import java.util.List;

interface Observer {
    void update(String message);
}

```

Class
```

class Customer implements Observer {
    private String name;

    public Customer(String name) {
        this.name = name;
    }

    public void update(String message) {
        System.out.println(name + " received notification: " + message);
    }
}

class OrderStatus {
    private List<Observer> observers = new ArrayList<>();

    public void addObserver(Observer observer) {
        observers.add(observer);
    }

    public void notifyObservers(String status) {
        for (Observer observer : observers) {
            observer.update(status);
        }
    }
}

```


Observer Class
```

public class ObserverPatternDemo {
    public static void main(String[] args) {
        OrderStatus order = new OrderStatus();
        Customer customer1 = new Customer("John");
        Customer customer2 = new Customer("Jane");

        order.addObserver(customer1);
        order.addObserver(customer2);

        order.notifyObservers("Your order has been shipped!");
    }
}

```



⸻

7. Adapter Pattern (External API Integrations)

Converts GraphQL API responses to REST format.

interface RestAPI {
    String getData();
}

class GraphQLService {
    public String executeQuery() {
        return "{ \"data\": \"GraphQL Response\" }";
    }
}

class GraphQLToRestAdapter implements RestAPI {
    private GraphQLService graphQLService;

    public GraphQLToRestAdapter(GraphQLService graphQLService) {
        this.graphQLService = graphQLService;
    }

    public String getData() {
        return "Converted to REST: " + graphQLService.executeQuery();
    }
}

public class AdapterPatternDemo {
    public static void main(String[] args) {
        GraphQLService graphQLService = new GraphQLService();
        RestAPI restAPI = new GraphQLToRestAdapter(graphQLService);
        System.out.println(restAPI.getData());
    }
}



⸻

8. CQRS Pattern (Command Query Responsibility Segregation)

Separates read and write operations.

interface QueryService {
    String getProductDetails(int id);
}

interface CommandService {
    void updateProductStock(int id, int quantity);
}

class ProductQueryService implements QueryService {
    public String getProductDetails(int id) {
        return "Product details for ID: " + id;
    }
}

class ProductCommandService implements CommandService {
    public void updateProductStock(int id, int quantity) {
        System.out.println("Updated stock for product ID: " + id + " with quantity: " + quantity);
    }
}

public class CQRSExample {
    public static void main(String[] args) {
        QueryService queryService = new ProductQueryService();
        CommandService commandService = new ProductCommandService();

        System.out.println(queryService.getProductDetails(101));
        commandService.updateProductStock(101, 50);
    }
}



⸻

These examples cover essential design patterns relevant to AEM development, e-commerce, and banking applications, providing modular, scalable, and maintainable architectures. 🚀 Let me know if you need any refinements!




# React

React is a JavaScript library for building user interfaces, primarily for web applications. It enables the creation of reusable UI components and efficiently updates and renders components when data changes. React follows a declarative approach and uses a virtual DOM to optimize rendering performance.

## Architecture

### Server vs. Client Components in React
	
 • Server Components: Rendered on the server, do not include client-side JavaScript, and improve performance by reducing bundle size.
 
 • Client Components: Rendered in the browser, allow for interactivity (e.g., event listeners, state management)
 
 • In Next.js (React framework), components are server-side by default but can be marked as client-side using "use client".

### Java in AEM (Adobe Experience Manager)

AEM is a Java-based CMS built on OSGi and Sling. Key Java concepts in AEM include:
	•	OSGi Bundles: Modular Java components that provide services.
	•	Sling Models: Java classes mapped to JCR nodes, allowing easy data retrieval.
	•	Servlets: Used for custom API endpoints within AEM.
	•	Workflows: Automated processes built using Java for content approvals and publishing.



### Headless CRM using React

A headless CRM (Customer Relationship Management) system separates the frontend from the backend. React can be used to build the UI while the CRM provides data via APIs.
	•	Key Concepts:
	•	Fetching CRM data via REST or GraphQL APIs.
	•	Using state management (Redux, React Query) for handling CRM interactions.
	•	Implementing authentication and authorization.
	•	Handling real-time updates with WebSockets or polling.



## React Functions

Yes, the code snippets are written in a format suitable for inclusion in a README.md file. However, to make them more readable and organized within a Markdown file, you can enhance the formatting by adding appropriate headers, explanations, and code block syntax. Here’s a revised version that you can directly use in your README.md:

⸻

React Functions

1. useState

Manages local component state. Ideal for UI-based state like form inputs.

import React, { useState } from 'react';

function Counter() {
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>Increment</button>
    </div>
  );
}

2. useReducer

An alternative to useState for managing complex state transitions (e.g., a counter with multiple actions).

import React, { useReducer } from 'react';

const initialState = { count: 0 };

function reducer(state, action) {
  switch (action.type) {
    case 'increment':
      return { count: state.count + 1 };
    case 'decrement':
      return { count: state.count - 1 };
    default:
      return state;
  }
}

function Counter() {
  const [state, dispatch] = useReducer(reducer, initialState);

  return (
    <div>
      <p>Count: {state.count}</p>
      <button onClick={() => dispatch({ type: 'increment' })}>Increment</button>
      <button onClick={() => dispatch({ type: 'decrement' })}>Decrement</button>
    </div>
  );
}

3. useContext

Shares state globally across components without prop drilling. Often used with React.createContext().

import React, { createContext, useContext, useState } from 'react';

const ThemeContext = createContext();

function ThemeProvider({ children }) {
  const [theme, setTheme] = useState('light');
  return (
    <ThemeContext.Provider value={{ theme, setTheme }}>
      {children}
    </ThemeContext.Provider>
  );
}

function ThemedComponent() {
  const { theme, setTheme } = useContext(ThemeContext);

  return (
    <div>
      <p>Current Theme: {theme}</p>
      <button onClick={() => setTheme(theme === 'light' ? 'dark' : 'light')}>
        Toggle Theme
      </button>
    </div>
  );
}

function App() {
  return (
    <ThemeProvider>
      <ThemedComponent />
    </ThemeProvider>
  );
}

4. useRef

Maintains references to DOM elements or persistent values across renders.

import React, { useRef } from 'react';

function FocusInput() {
  const inputRef = useRef(null);

  const focusInput = () => {
    inputRef.current.focus();
  };

  return (
    <div>
      <input ref={inputRef} type="text" placeholder="Focus me!" />
      <button onClick={focusInput}>Focus Input</button>
    </div>
  );
}

5. useMemo

Optimizes performance by memoizing values based on dependencies.

import React, { useMemo, useState } from 'react';

function ExpensiveCalculation() {
  const [count, setCount] = useState(0);

  // Only recompute when count changes
  const expensiveValue = useMemo(() => {
    console.log('Computing expensive value...');
    return count * 2;
  }, [count]);

  return (
    <div>
      <p>Expensive Value: {expensiveValue}</p>
      <button onClick={() => setCount(count + 1)}>Increment</button>
    </div>
  );
}

6. useCallback

Memoizes functions to prevent unnecessary re-renders in child components.

import React, { useCallback, useState } from 'react';

function ExpensiveComponent({ onClick }) {
  console.log('Rendering ExpensiveComponent');
  return <button onClick={onClick}>Click me</button>;
}

function Parent() {
  const [count, setCount] = useState(0);

  // Memoize the function to prevent re-renders
  const handleClick = useCallback(() => {
    setCount(count + 1);
  }, [count]);

  return (
    <div>
      <ExpensiveComponent onClick={handleClick} />
      <p>Count: {count}</p>
    </div>
  );
}

7. useEffect

Performs side effects like fetching data or setting up event listeners.

import React, { useEffect, useState } from 'react';

function FetchData() {
  const [data, setData] = useState(null);

  useEffect(() => {
    // Fetching data on mount
    fetch('https://api.example.com/data')
      .then(response => response.json())
      .then(data => setData(data));
  }, []);  // Empty dependency array means it runs only once on mount

  return <div>{data ? JSON.stringify(data) : 'Loading...'}</div>;
}

8. useLayoutEffect

Similar to useEffect, but runs synchronously after DOM mutations.

import React, { useLayoutEffect, useRef } from 'react';

function LayoutEffectComponent() {
  const divRef = useRef(null);

  useLayoutEffect(() => {
    // This runs after the DOM is updated
    console.log(divRef.current.getBoundingClientRect());
  }, []);

  return <div ref={divRef}>Hello, world!</div>;
}



⸻

React State Management Functions

State Management Functions

Function	Usage
useState	Manages local component state. Ideal for UI-based state like form inputs.
useReducer	Alternative to useState for complex state logic (e.g., state transitions).
useContext	Shares state between components without prop drilling. Often used with React.createContext().
useRef	Maintains references to DOM elements or persistent values without re-renders.
useMemo	Optimizes performance by memoizing values based on dependencies.
useCallback	Memoizes functions to prevent unnecessary re-renders in child components.
useEffect	Performs side effects like fetching data or setting up event listeners.
useLayoutEffect	Similar to useEffect but runs synchronously after DOM mutations.



⸻

Key React Patterns

Higher-Order Components (HOC)

A function that takes a component and returns an enhanced version of it.

function withAuth(Component) {
  return function AuthHOC(props) {
    const isAuthenticated = useAuth();
    return isAuthenticated ? <Component {...props} /> : <Redirect to="/login" />;
  };
}

Render Props

A pattern where a function is passed as a prop to dynamically render content.

function MouseTracker({ render }) {
  const [mousePosition, setMousePosition] = useState({ x: 0, y: 0 });

  useEffect(() => {
    const handleMouseMove = (e) => {
      setMousePosition({ x: e.clientX, y: e.clientY });
    };
    window.addEventListener('mousemove', handleMouseMove);
    return () => window.removeEventListener('mousemove', handleMouseMove);
  }, []);

  return <div>{render(mousePosition)}</div>;
}

function App() {
  return (
    <MouseTracker render={(position) => <p>Mouse Position: {position.x}, {position.y}</p>} />
  );
}

Compound Components

Groups related components together for better composition (e.g., <Accordion> with <AccordionItem>).

function Accordion({ children }) {
  const [openIndex, setOpenIndex] = useState(null);

  const handleToggle = (index) => {
    setOpenIndex(index === openIndex ? null : index);
  };

  return <div>{React.Children.map(children, (child, index) => 
    React.cloneElement(child, { index, openIndex, onToggle: handleToggle })
  )}</div>;
}

function AccordionItem({ index, openIndex, onToggle, children }) {
  return (
    <div>
      <button onClick={() => onToggle(index)}>Toggle</button>
      {openIndex === index && <div>{children}</div>}
    </div>
  );
}

function App() {
  return (
    <Accordion>
      <AccordionItem>Content 1</AccordionItem>
      <AccordionItem>Content 2</AccordionItem>
    </Accordion>
  );
}

Controlled Components

Form inputs are controlled by the React state instead of the DOM.

function ControlledForm() {
  const [value, setValue] = useState('');

  const handleChange = (event) => {
    setValue(event.target.value);
  };

  return <input type="text" value={value} onChange={handleChange} />;
}

Uncontrolled Components

Form elements that rely on the DOM state using useRef.

function UncontrolledForm() {
  const inputRef = useRef();

  const handleSubmit = () => {
    alert('Form submitted with value: ' + inputRef.current.value);
  };

  return (
    <div>
      <input ref={inputRef} type="text" />
      <button onClick={handleSubmit}>Submit</button>
    </div>
  );
}

Context API

Shares state globally without prop drilling.

const MyContext = createContext();

function MyComponent() {
  const value = useContext(MyContext);
  return <div>{value}</div>;
}

function App() {
  return (
    <MyContext.Provider value="Hello from Context">
      <MyComponent />
    </MyContext.Provider>
  );
}

Suspense & Lazy Loading

Dynamically loads components when needed using React.lazy().

import React, { Suspense } from 'react';

const LazyComponent = React.lazy(() => import('./LazyComponent'));

function App() {
  return (
    <Suspense fallback={<div>Loading...</div>}>
      <LazyComponent />
    </Suspense>
  );
}

Portals

Renders components outside the main React tree, useful for modals.

import ReactDOM from 'react-dom';

function Modal({ children }) {
  return ReactDOM.createPortal(
    <div className="modal">
      {children}
    </div>,
    document.getElementById('modal-root')
  );
}



⸻

This format is Markdown-friendly and should render well on platforms like GitHub or other markdown parsers.




#### **Use Case**
Imagine an e-commerce website where the header, navigation bar, and product list can be loaded quickly, but customer reviews take longer due to database queries. Using SSSR, the server can send the header and navigation first, allowing users to start interacting with the page while the customer reviews are still being fetched and streamed in later.

### **Other Similar Rendering Techniques**

#### 1. **CSR (Client-Side Rendering)**
- Renders content entirely in the browser using JavaScript.
- Requires downloading and executing the JavaScript bundle before displaying anything.
- Example: Single Page Applications (SPAs) using React.

#### 2. **SSR (Server-Side Rendering)**
- The server generates the full HTML page before sending it to the client.
- Reduces initial load time but can block rendering until all data is fetched.
- Uses `ReactDOMServer.renderToString()`.

#### 3. **ISR (Incremental Static Regeneration)**
- Combines static site generation (SSG) with dynamic updates at runtime.
- Pages are generated at build time but can be updated in response to user requests.
- Used in Next.js with `getStaticProps` and revalidation intervals.

#### 4. **RSC (React Server Components)**
- Introduced in React 18, allowing components to be rendered on the server while keeping stateful and interactive components client-side.
- Reduces bundle size by offloading logic to the server.
- Compatible with Next.js App Router (`app/` directory).

### **Conclusion**
SSSR is a powerful rendering approach for improving performance and user experience, particularly for data-heavy applications where certain content can be loaded progressively. Understanding when to use SSSR versus other rendering strategies like SSR, CSR, or ISR is key to optimizing React applications.


⸻



### 2. SSSR (Streaming Server-Side Rendering)

Streaming Server-Side Rendering (SSSR) is an advanced React rendering technique that enables a server to send parts of a page to the client as soon as they are ready, rather than waiting for the entire page to be fully generated. This technique improves performance by:

- **Reducing Time-to-First-Byte (TTFB):** Content is streamed progressively, allowing users to see meaningful content sooner.
- **Enabling progressive hydration:** Components are rendered and hydrated as they load, improving interactivity.
- **Utilizing React’s built-in streaming APIs:** `ReactDOMServer.renderToNodeStream()` (pre-React 18) and `renderToPipeableStream()` (React 18+).



Explaining SSSR 
	1.	Start with a high-level definition:
“Streaming Server-Side Rendering (SSSR) is an advanced React rendering technique where the server streams parts of the page to the client as they become available, instead of waiting for the entire page to be generated.”
	2.	Mention the key benefits:
	•	Reduces Time-to-First-Byte (TTFB) by sending content progressively.
	•	Allows progressive hydration, making the page interactive faster.
	•	Uses renderToPipeableStream() (React 18+) for efficient streaming.
	3.	Give a practical example:
	•	Imagine an e-commerce website where the navigation and product list load instantly, while customer reviews are still fetching in the background. SSSR allows the user to interact with the page while waiting for additional content to load progressively.
	4.	Differentiate from traditional SSR:
	•	SSR waits for the entire page to be ready before sending it to the client, which can slow down performance if data fetching takes time. SSSR improves this by streaming content as soon as it’s available.

⸻


How to Present This in an Interview
	•	Keep it structured: Definition → Benefits → Example → Comparison
	•	Use real-world analogies: “SSSR is like watching a video that streams instantly instead of waiting for the whole file to download.”
	•	Connect it to React concepts: “It works well with Suspense for streaming dynamic content.”
	•	Use keywords: Streaming, Progressive Hydration, React 18, Performance Optimization.



# GraphQL
 

## AEM Headless Architecture with GraphQL

### 1. Understanding AEM Headless Architecture

AEM’s headless CMS capability allows developers to separate the content management backend (AEM) from the frontend, which can be built using React, Next.js, Vue.js, or any other framework. Instead of serving HTML pages, AEM exposes structured content via APIs (GraphQL or REST), which frontend applications consume dynamically.

#### Key Features of AEM Headless:
- **Content Fragment Models**: Define reusable content structures (e.g., article, product, event).
- **Content Delivery via APIs**: Fetch structured content using GraphQL API or REST API.
- **Decoupled Frontend**: React, Next.js, or any frontend framework can consume AEM content.
- **Efficient Content Management**: Authors can use AEM’s UI while developers work with APIs.

---

### 2. Why Use GraphQL in AEM Headless?

AEM supports GraphQL as an efficient alternative to REST for fetching content.

#### Advantages of GraphQL in AEM:
✅ Fetch only the required data (no over-fetching or under-fetching).  
✅ Retrieve nested content in a single request (better performance).  
✅ Reduce API calls compared to REST.  
✅ Supports complex queries with filters, pagination, and sorting.  

---

### 3. How AEM GraphQL Works?

AEM’s GraphQL API provides access to Content Fragments (CFs) stored in the JCR (Java Content Repository).

#### Steps to Use AEM GraphQL API:

1. **Enable GraphQL Endpoint**  
   - Navigate to AEM Configuration Console (`/system/console/configMgr`).  
   - Enable GraphQL servlet under Adobe Granite GraphQL Endpoint.

2. **Create a Content Fragment Model**  
   - Go to **AEM > Tools > Assets > Content Fragment Models**.  
   - Define a model (e.g., Blog, Product, Event) with necessary fields (title, description, image, author).

3. **Create Content Fragments**  
   - Go to **AEM > Assets > Content Fragments** and create fragments based on the model.

4. **Construct GraphQL Queries**  
   - AEM provides a GraphQL playground (`/graphql/execute.json/{endpoint}`).
   - Example GraphQL Query to fetch blog content:

```graphql
{
  blogByPath(_path: "/content/dam/blogs/sample") {
    item {
      title
      description
      author {
        name
        bio
      }
      image {
        _path
      }
    }
  }
}
```

5. **Consume AEM GraphQL API in React**  
   - Use Apollo Client or Fetch API to get AEM content in a React app.
   - Example using Apollo Client in React:

```javascript
import { ApolloClient, InMemoryCache, gql } from "@apollo/client";

const client = new ApolloClient({
  uri: "https://your-aem-instance/graphql/execute.json/your-endpoint",
  cache: new InMemoryCache(),
});

const GET_BLOGS = gql`
  {
    blogByPath(_path: "/content/dam/blogs/sample") {
      item {
        title
        description
        author {
          name
          bio
        }
        image {
          _path
        }
      }
    }
  }
`;

client.query({ query: GET_BLOGS }).then((response) => console.log(response.data));
```

---

### 4. AEM Headless + GraphQL + React Architecture

📌 **Architecture Flow:**  
1️⃣ Content authors create & manage Content Fragments in AEM.  
2️⃣ AEM exposes content via GraphQL API.  
3️⃣ Frontend (React/Next.js) fetches content dynamically.  
4️⃣ Data is displayed on the frontend with real-time updates.  

**[AEM CMS] ---> [GraphQL API] ---> [React Frontend]**

---

### 5. Advanced GraphQL Features in AEM

🔹 **Filtering:**
```graphql
{
  blogByPath(_path: "/content/dam/blogs/sample") {
    item {
      title
      description
    }
  }
}
```

🔹 **Pagination:**
```graphql
{
  blogList(limit: 5, offset: 10) {
    items {
      title
    }
  }
}
```

🔹 **Nested Queries:**
```graphql
{
  productByPath(_path: "/content/dam/products/shirt") {
    item {
      name
      price
      category {
        name
      }
      reviews {
        user
        rating
      }
    }
  }
}
```

---

### 6. Deployment Considerations

✅ **AEM as a Cloud Service**: Use AEM Headless API on Adobe Experience Manager as a Cloud Service for scalability.  
✅ **Caching & Performance**: Use CDN (Akamai, CloudFront) and GraphQL caching for faster responses.  
✅ **Authentication**: Secure GraphQL API with OAuth/JWT for protected content.  
✅ **SSG/ISR with Next.js**: Optimize performance using Static Site Generation (SSG) or Incremental Static Regeneration (ISR).  

---

### 7. Summary

| **Feature** | **AEM Headless (GraphQL)** |
|------------|---------------------------|
| **API Type** | GraphQL |
| **Data Structure** | Content Fragments |
| **Frontend** | React, Next.js, Vue.js |
| **Benefits** | Optimized queries, reduced API calls, faster performance |
| **Authentication** | OAuth, JWT |


---



