# Intro-To-Backend-Architectures

## Types of Backend Architectures
- Monolithic Architecture
- Service Oriented Architecture (SOA)
- Microservice Architecture

## Monolithic Architectures
### Characteristics
- Single Build
  - Layers such as UI, Business Layer, and Data Access Layers are bundled together into a single build
- Tightly Coupled
  - Enables very high cohesion and reduces security concerns but reduces the maintainability as the app grows
  - Simpler to test but the whole build needs to be tested even if a single piece of functionality is changed
  - Release cycles tend to be longer because of long regression testing activities
  - Takes a very long time to deploy and configure
- Lack of Flexibility
  - Cannot scale a piece of functionality that is more frequently used/accessed independent of others
  - Horizontally scalable but is usually associated with higher cost implications
  - Introduces dependency on the language/framework chosen
  - Introduces a notion of single point of failure

### When to use?
- Suited for small/simple apps supporting only a finite set of features, preferably for in-house tasks etc.
- Easy to build and maintain with a small team
- No demand for scalability and flexibility

## Service Oriented Architecture (SOA)
- SOA is an architectural approach that structures apps into **isolated** and **reusable** services
- The services interact through common communication standards such as **Enterprise Service Bus (ESB)**

### Characteristics
- Loosely Coupled
- ESB
  -  Integration Patterns Evolution
    * Point to Point
    * Hub and Spoke
    * Enterprise Service Bus
- Service Providers and Consumers
- SOAP & WSDL
  - Simple Object Access Protocol
  - Platform and Language Independent
    * App1 (ASP .Net) <<---- XML ---->> App2 (Java)
  - It is an XML-based protocol for accessing web-services over HTTP
  - Defines the format for sending and receiving messages with the help of WSDL
    * Web Services Description Language
    * It is an XML notation for describing a web service
    * A WSDL definition is divided into separate sections that specify the logical interface and the physical details of a web service
      - Logical Interface
        * Describes the interface (**<type>** element) that is provided by the web service provider
        * Tells a client how to compose (**<message>** element) a web service request
      - Physical Details
        * Endpoint information (**<portType>** element) that describe the set of operations provided by a web service
        * Binding information, which specifies how the SOAP payload is represented (**<binding>** element) and which transport is used
  - Enables apps to easily connect to remove services and invoke remote methods
  - Structure
    * SOAP Message (an XML document)
      - SOAP Part
        * SOAP Enveloper
          - SOAP Header     (Message Metadata Info - Optional   )
            * Header 1
            * Header 2
          - SOAP Body
            * XML Content   (Message i.e., Request/Response Data)
            * SOAP Fault    (Error/Status Info - Optional       )
- Reusable Service Interfaces
- Faster Time to Market
- Greater Business Agility
- Leverage Legacy Functionality
- Improved Collaboration b/w Business & IT

### When to use?
- Applications with complex operations with multiple participants services
- Participating services are build using different platforms or programming languages

## Microservice Architecture (MSA)
- Microservices is an architectural approach where apps are broken down into their smallest components, independent of each other
- Each of these independent components, or processes, is a microservice
  - **Layer #1** FrontEnd
  - **Layer #2** Backend
    - Aggregation Layer (ex: BFF, GraphQL, etc.)
    - Product Service, Payment Service etc.
  - **Layer #3** Database

### Characteristics
- Lightweight Protocols (ex: REST, gRPC)
  * REST stands for Representational State Transfer
    - It is a set of guidelines/architecture that needs to be following when API is created
    - Using REST API, we can send requests and receive responses using the HTTP protocol
    - API is an application programming interface that is created on the server and allows the clients to communicate
    - Characteristics of a REST API
      * **Client-Server**    : This constraint operates on the concept that the client and the server should be separate from each other and allowed to evolve individually.
      * **Stateless**        : REST APIs are stateless, meaning that calls can be made independently of one another, and each call contains all the data necessary to complete itself successfully.
      * **Cache**            : Because a stateless API can increase request overhead by handling large loads of incoming and outbound calls, a REST API should be designed to encourage the storage of cacheable data.
      * **Uniform Interface**: The key to the decoupling client from server is having a uniform interface that allows independent evolution of the application without having the application’s services, or models and actions, tightly coupled to the API layer itself.
      * **Layered System**   : REST APIs have different layers of their architecture working together to build a hierarchy that helps create a more scalable and modular application.
      * **Code on Demand**   : Code on Demand allows for code or applets to be transmitted via the API for use within the application.
    - Pros
      * The business logic is decoupled from presentation. So you can change one without impacting the other.
      * All messages exchanged between client and server has all the context needed to know what to do with the message. This visibility has several benefits: you can route a message where ever you want depending on its contents and any server can service a request. So you a just scale your server by creating several instances of it. You don't need to send all messages from the same client or user to the same server. And if you want to tune the backend, you can rout messages to different server depending on the message. For example, having a CPU intensive request to one server and a memory intensive one to another.
      * It is cacheable, so if you were worried about latency you save bandwidth caching responses from the server.
      * It has a uniform interface based on hypermedia (you know, that HATEOAS thing). The great thing about this is that you can greatly improve decoupling between client and server. If server responses contain hypermedia to all referenced resources and available actions within the context of the last request, the client does not need to know much about the server but an entry point and a few conventions about the hypermedia. Properly implemented, you could change many things in the server side without rewriting a single line in the client.
      * You can put several layers of components between client and server, for routing purposes, load balancing, caching or whatever you need. Of course, it adds latency but also lots of flexibility. And if you change a layer, only the previous layer could be impacted, so propagation of change effects is limited.
    - Cons
      * RESTful APIs are built on URIs referencing resources. URIs are convenient for http requests and caching, but they are a poor fit for resources that are not naturally organized or accessed in simple hierarchy.
      * For example, a request like, "Return all updated records from the last 3 hours containing the word cat" does not lend itself to being expressed as a path, so is likely to have to be implemented with some combination of URI path, query parameters, and perhaps request body.
      * RESTful APIs typically rely on a few http methods (GET, POST, PUT, DELETE, perhaps PATCH), but many common client/server operations can only awkwardly be shoehorned into the standard methods. "Move expired documents to the archive folder" is an example of an application-specific custom verb that's outside typical http methods and CRUD operations.
      * The stateless nature is in practice often a myth. REST APIs end up depending on headers for state (such as to route subsequent requests to the same back-end server that handled the previous update, or for authentication.) Use of headers is clumsy and ties the API to http as transport.
      * The growth of RESTful APIs can be attributed to the success of http and related internet infrastructure. A more general approach to requests and responses, such as using JSON-RPC, allows for more expressive, less cumbersome API design.
      * REST is appropriate for public URLs, but it is not suitable for private data passage between client and server as it does not provide security such as SOAP.
      * It is not possible to page through thousands of products directly in the REST API.
  * REST API Authentication Mechanisms
    - HTTP Authentication Schemes - Basic Authentication
      * Client requests are added with base64 encoded username:password string in the header when making requests
      * Not recommended due to its inherent security vulnerabilities
      * Authorization: Basic Base64_Encode(username:password)
    - HTTP Authentication Schemes - Bearer Authentication
      * Client requests are added with token in the Authorization header when making requests to protected resources
      * Bearer authentication is also called token authentication, and it should be only used over HTTPS(SSL)
      * Authorization: Bearer <token>
    - API Keys
      * Client request header would have a unique generated value (API Key), indicating that the user is known/trusted
      * When the user attempts to re-enter the system, their unique key is used to prove themselves
    - OAuth Token
      * When the user logins to a system, they are redirected to authentication server, which will either reject or allow the authentication
      * From here, the token is provided to the user, and then to the requester
      * Each request there on should have this token attached to the request and can then be checked each time by the requester for validation defined scope/validity expiry
- Ready for Market Faster
- Loosely Coupled
- Highly Scalable
- Easy to Deploy
- Distributed
- Resilient
- DevOps
- CI/CD

### When to use?
- Needs a loosely coupled system and individual components needs to be scaled independent of others
- Better for smaller and web-based applications
- Faster time to market

## How are SOA and Microservice architectures different from each other?
| Area                | SOA                                                    | Microservices                                                                               |
|:--------------------|:-------------------------------------------------------|:--------------------------------------------------------------------------------------------|
| Scope               | Enterprise Scope - Better for large scale integrations | Application Scope - Better for smaller and web-based applications                           |
| Governance          | Common governance protocols across teams               | Requires collaboration between teams                                                        |
| Deployment          | Less flexible                                          | Quick and easy                                                                              |
| Communication       | Common communication mechanism (ESB)                   | Communicates through an API layer                                                           |
| Data storage        | Involves sharing data storage between services         | Each service can have an independent data storage                                           |
| Data Duplication    | No need for data synchronization patterns              | Special care needs to be taken care of since data is local (stateless) to each microservice |
| Synchronous Calls   | Uses synchronous/blocking calls using SOAP and AMQP    | Asynchronous/Non-blocking paradigm is preferred using REST and JMS                          |
| Interoperability    | Heterogeneous Messaging Protocols (SOAP)               | Lightweight messaging protocol (HTTP/REST)                                                  |
| Component Sharing   | Frequently involves component sharing                  | Typically does not involve component sharing                                                |
| Service Granularity | Larger, more modular services                          | Fine-grained services                                                                       |

## Microservices Design Patterns
### Aggregator Pattern
- The Client needs details (smaller details) from each of the services A, B & C
- The Aggregator component calls individual services, consolidates the results and sends the result back to the Client
```
Load Balancer  ┌→ Service A ←→ Cache ←→ DB
       ╎       ╎
       ↓       ╎
   Aggregator ←┼→ Service B ←→ Cache ←→ DB
               ╎
               ╎
               └→ Service C ←→ Cache ←→ DB
```

### Proxy Pattern
- The Client needs details from a specific service A, B or C
- The Proxy component calls appropriate service and sends the desired result back to the Client
- Here, the Proxy service acts as an interface and provides an extra layer of security for all the API Calls
```
Load Balancer  ┌→ Service A ←→ Cache ←→ DB
       ╎       ╎
       ↓       ╎
     Proxy    ←┼→ Service B ←→ Cache ←→ DB
               ╎
               ╎
               └→ Service C ←→ Cache ←→ DB
```

### Chained Pattern
- This is one of the composition patterns where client requests are sent directly to the services
- All the services will be chained up in a manner such that the output of one service will be the input of the next one
- Here, Service A receives user requests, and other services output are chained
```
Load Balancer --→ Service A ←→ Cache ←→ DB
                     ↑
                     ↓
                  Service B ←→ Cache ←→ DB
                     ↑
                     ↓
                  Service C ←→ Cache ←→ DB
```

### Shared Data Pattern
- In this pattern, to reduce data duplication/inconsistency issues, each microservice can have shared data between them
- In this example, Services B and C share the same database
```
Service A ←→ Cache ←→ DB
    ↑
    ╎
    ↓
┌-----------------------------┐
╎   Service B ←┐              ╎
╎      ↑       ╎              ╎
╎      ╎       ├→ Cache ←→ DB ╎ 
╎      ↓       ╎              ╎  
╎   Service C ←┘              ╎
└-----------------------------┘
```

### Asynchronous Messaging Pattern
- In this pattern, Asynchronous Messaging is used to service the requests concurrently
- In this example, Service A and Service B can communicate with each other concurrently
- The request from the client can be directly sent to Services A and B concurrently
- RabbitMA or Kafka are a few examples of message queues
```
               ┌-------┐
Service A ←--→ ╎ Queue ╎ ←--→ Service B
    ↑          └-------┘          ↑    
    ↓                             ↓
  Cache                         Cache
    ↑                             ↑
    ↓                             ↓
 Database                      Database
```

### Anti-corruption Layer Pattern
- In this pattern, the anti-corruption layer is introduced between systems that transforms communications between the two systems
- This allows other systems to remain unaffected irrespective of its design, communication, technological approach, etc.
- Here, Subsystem A can communicate with Subsystem C irrespective of its design, technological approach, etc.
```
     LEGACY SYSTEMS
┌-------------------------┐  ┌------------┐
╎ Service A ←-------------┼-→╎            ╎    MODERN API  
╎     ↑                   ╎  ╎            ╎   ┌-----------┐
╎     ↓                   ╎  ╎            ╎   ╎ Service C ╎
╎   Cache                 ╎  ╎            ╎   ╎     ↑     ╎
╎     ↑                   ╎  ╎   Anti     ╎   ╎     ↓     ╎
╎     ↓       Service B ←-┼-→╎ Corruption ╎←--┼-→ Cache   ╎
╎  Database       ↑       ╎  ╎   Layer    ╎   ╎     ↑     ╎
╎                 ↓       ╎  ╎            ╎   ╎     ↓     ╎
╎               Cache     ╎  ╎            ╎   ╎  Database ╎
╎                 ↑       ╎  ╎            ╎   └-----------┘         
╎                 ↓       ╎  ╎            ╎
╎              Database   ╎  ╎            ╎
└-------------------------┘  └------------┘
```

## API Gateway
- Is a component that sits between the clients and a collection of backend microservices
- Responsible for request routing, protocol translation etc.
- Can often handle a request by invoking multiple microservices and aggregating the results
- It can also be used for rate limiting (throttling) and authentication
- A monitoring tool analyzing how the APIs are being used can be plugged into the gateway
```
                                        REST API
                                  ┌-------------------┐
                               ┌-→╎ Shipping Service  ╎
                               ╎  └-------------------┘ 
                               ╎ 
                               ╎        REST API
┌--------┐    ┌-------------┐  ╎  ┌-------------------┐
╎ CLIENT ╎←--→╎ API Gateway ╎←-┼-→╎ Order Service     ╎  
└--------┘    └-------------┘  ╎  └-------------------┘ 
                               ╎ 
                               ╎        REST API
                               ╎  ┌-------------------┐
                               └-→╎ Inventory Service ╎
                                  └-------------------┘ 
```
