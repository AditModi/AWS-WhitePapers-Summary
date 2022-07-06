# Running Containerized Microservices on AWS

- This whitepaper is intended for architects and developers who want to run containerized applications at scale in production on Amazon Web Services (AWS). 
- This document provides guidance for application lifecycle management, security, and architectural software design patterns for container-based applications on AWS.

- It also discusses architectural best practices for adoption of containers on AWS, and how traditional software design patterns evolve in the context of containers. 

- It leverages Martin Fowler’s principles of microservices and map them to the twelve-factor app pattern and real-life considerations. 
- After reading this paper, you will have a starting point for building microservices using best practices and software design patterns.
- Microservices are an architectural and organizational approach to software development in which software is composed of small, independent services that communicate to each other.
- There are diﬀerent ways microservices can communicate, but the two commonly used protocols are HTTP request/response over well-deﬁned APIs, and lightweight asynchronous messaging.
- Microservices architectures make applications easier to scale and faster to develop.
- This enables innovation and accelerates time-to-market for new features. Containers also provide isolation and packaging for software, and help you achieve more deployment velocity and resource density.

- As proposed by Martin Fowler, the characteristics of a microservices architecture include the following:
  - Componentization via services
  - Organized around business capabilities
  - Products not projects
  - Smart endpoints and dumb pipes
  - Decentralized governance
  - Decentralized data management
  - Infrastructure automation
  - Design for failure
  - Evolutionary design

- The twelve factors are a set of best practices for building modern applications that are optimized for cloud computing. The twelve factors cover four key areas: deployment, scale, portability, and architecture:

 1. Codebase - One codebase tracked in revision control, many deploys
 2. Dependencies - Explicitly declare and isolate dependencies
 3. Conﬁg - Store conﬁgurations in the environment
 4. Backing services - Treat backing services as attached resources
 5. Build, release, run - Strictly separate build and run stages
 6. Processes - Execute the app as one or more stateless processes
 7. Port binding - Export services via port binding
 8. Concurrency - Scale out via the process model
 9. Disposability - Maximize robustness with fast startup and graceful shutdown
 10. Dev/prod parity - Keep development, staging, and production as similar as possible
 11. Logs - Treat logs as event streams
 12. Admin processes - Run admin/management tasks as one-oﬀ processes


## Componentization Via Services

- In a microservices architecture, software is composed of small independent services that communicate over well-deﬁned APIs.
- An analogy can be drawn to the Walkman portable audio cassette players that were popular in the 1980s: batteries bring power, audio tapes are the medium, headphones deliver output, while the main tape player takes input through key presses. Using them together plays music.
- Similarly, microservices need to be decoupled, and each should focus on one functionality. Additionally, a microservices architecture allows for replacement or upgrade. 

- Using the Walkman analogy, if the headphones are worn out, you can replace them without replacing the tape player. 
- Through modularization, microservices oﬀer developers the freedom to design each feature as a black box. That is, microservices hide the details of their complexity from other components. 
- Any communication between services happens by using well-deﬁned APIs to prevent implicit and hidden dependencies.
- Container images allow for modularity in services. 
- They are constructed by building functionality onto a base image. 
- Developers, operations teams, and IT leaders should agree on base images that have the security and tooling proﬁle that they want. These images can then be shared throughout the organization as the initial building block. 
- Replacing or upgrading these base images is as simple as updating the FROM ﬁeld in a Dockerﬁle and rebuilding, usually through a Continuous Integration/Continuous Delivery (CI/CD) pipeline.

- Here are the key factors from the twelve-factor app pattern methodology that play a role in componentization:
  - Dependencies (explicitly declare and isolate dependencies) – Dependencies are self-contained within the container and not shared with other services.
  - Disposability (maximize robustness with fast startup and graceful shutdown) – Disposability is leveraged and satisﬁed by containers that are easily pulled from a repository and discarded when they stop running.
  - Concurrency (scale out via the process model) – Concurrency consists of tasks or pods (made of containers working together) that can be auto scaled in a memory- and CPU-eﬃcient manner.


## Organized Around Business Capabilities
- Before microservices, system architecture would be organized around technological capabilities such as user interface, database, and server-side logic. 
- In a microservices-based approach, as a best practice, each development team owns the lifecycle of its service all the way to the customer. 
- For example, a recommendations team might own development, deployment, production support, and collection of customer feedback.
- Organizations which design systems ... are constrained to produce designs which are copies of the communication structures of these organizations. "Conway's Law"
- When architecture and capabilities are organized around atomic business functions, dependencies between components are loosely coupled. As long as there is a communication contract between services and teams, each team can run at its own speed. 
- With this approach, the stack can be polyglot, meaning that developers are free to use the programming languages that are optimal for their component.
- For example, the user interface can be written in JavaScript or HTML5, the backend in Java, and data processing can be done in Python.

- The following are key factors from the twelve-factor app pattern methodology that play a role in organizing around capabilities:
   - Codebase (one codebase tracked in revision control, many deploys) –Each microservice owns its own codebase in a separate repository and throughout the lifecycle of the code change.
   - Build, release, run (strictly separate build and run stages) – Each microservice has its own deployment pipeline and deployment frequency. This allows the development teams to run microservices at varying speeds so they can be responsive to customer needs.
   - Processes (execute the app as one or more stateless processes) – Each microservice does one thing and does that one thing really well. The microservice is designed to solve the problem at hand in the best possible manner.
   - Admin processes (run admin/management tasks as one-oﬀ processes) – Each microservice has its own administrative or management tasks so that it functions as designed. 

- To achieve a microservices architecture that is organized around business capabilities, use popular microservices design patterns. 
- A design pattern is a general, reusable solution to a commonly occurring problem within a giving context.

- Popular miscroservice design patterns include:
   - Aggregator Pattern – A basic service which invokes other services to gather the required information or achieve the required functionality. This is beneﬁcial when you need an output by combining data from multiple microservices.
   - API Gateway Design Pattern – API Gateway also acts as the entry point for all the microservices and creates ﬁne-grained APIs for diﬀerent types of clients. It can fan out the same request to multiple microservices and similarly aggregate the results from multiple microservices.
   - Chained or Chain of Responsibility Pattern – Chained or Chain of Responsibility Design Patterns produces a single output which is a combination of multiple chained outputs. object.
   - Asynchronous Messaging Design Pattern – In this type of microservices design pattern, all the services can communicate with each other, but they do not have to communicate with each other sequentially and they usually communicate asynchronously.

   - Database or Shared Data Pattern – This design pattern will enable you to use a database per service and a shared database per service to solve various problems. These problems can include duplication of data and inconsistency, diﬀerent services have diﬀerent kinds of storage requirements, few business transactions can query the data, and with multiple services and de-normalization of data.
   - Event Sourcing Design Pattern – This design pattern helps you to create events according to change of your application state.
   - Command Query Responsibility Segregator (CQRS) Design Pattern – This design pattern enables you to divide the command and query. Using the common CQRS pattern, where the command part will handle all the requests related to CREATE, UPDATE, DELETE while the query part will take care of the materialized views.
   - Circuit Breaker Pattern – This design pattern enables you to stop the process of the request and response when the service is not working. For example, when you need to redirect the request to a diﬀerent service after certain number of failed request intents.
   - Decomposition Design Pattern – This design pattern enables you to decompose an application based on business capability or on based on the sub-domains.


## Products Not Projects 
- Companies that have mature applications with successful software adoption and who want to maintain and expand their user base will likely be more successful if they focus on the experience for their customers and end users.
- To stay healthy, simplify operations, and increase eﬃciency, your engineering organization should treat software components as products that can be iteratively improved and that are constantly evolving.
- When software architecture is broken into small microservices, it becomes possible for each microservice to be an individual product. 
- For internal microservices, the end user of the product is another team or service. 
- For an external microservice, the end user is the customer.
- The core beneﬁt of treating software as a product is improved end-user experience. 
- When your organization treats its software as an always-improving product rather than a one-oﬀ project, it will produce code that is better architected for future work.
- The following are key factors from the twelve-factor app pattern methodology that play a role in adopting a product mindset for delivering software:
   - Build, release, run – Engineers adopt a devops culture that allows them to optimize all three stages.
   - Conﬁg – Engineers build better conﬁguration management for software due to their involvement with how that software is used by the customer.
   - Dev/prod parity – Software treated as a product can be iteratively developed in smaller pieces that take less time to complete and deploy than long-running projects, which enables development and production to be closer in parity.

## Smart Endpoints and Dumb Pipes

- There are two primary forms of communication between services:
    - Request/Response – One service explicitly invokes another service by making a request to either store data in it or retrieve data from it. For example, when a new user creates an account, the user service makes a request to the billing service to pass oﬀ the billing address from the user’s proﬁle so that that billing service can store it.
    - Publish/Subscribe – Event-based architecture where one service implicitly invokes another service that was watching for an event. For example, when a new user creates an account, the user service publishes this new user signup event and the email service that was watching for it is triggered to email the user asking them to verify their email.
   
- The core beneﬁt of building smart endpoints and dumb pipes is the ability to decentralize the architecture, particularly when it comes to how endpoints are maintained, updated, and extended.
- One goal of microservices is to enable parallel work on diﬀerent edges of the architecture that will not conﬂict with each other. 
- Building dumb pipes enables each microservice to encapsulate its own logic for formatting its outgoing responses or supplementing its incoming requests.
- The following are the key factors from the twelve-factor app pattern methodology that play a role in building smart endpoints and dumb pipes:
   - Port Binding – Services bind to a port to watch for incoming requests and send requests to the port of another service. The pipe in between is just a dumb network protocol such as HTTP.
   - Backing services – Dumb pipes allow a background microservice to be attached to another microservice in the same way that you attach a database.
   - Concurrency – A properly designed communication pipeline between microservices allows multiple microservices to work concurrently. For example, several observer microservices may respond and begin work in parallel in response to a single event produced by another microservice.

## Decentralized Governance
- Decentralized governance is an approach that works well alongside microservices to enable engineering organizations to tackle this challenge. 
- Traﬃc lights are a great example of decentralized governance. 
- City traﬃc lights may be timed individually or in small groups, or they may react to sensors in the pavement.
- Decentralized governance helps remove potential bottlenecks that would prevent engineers from being able to develop the best code to solve business problems.
- Decentralized governance means that each team can use its expertise to choose the best tools to solve their speciﬁc problem. 
- Forcing all teams to use the same database, or the same runtime language, isn’t reasonable because the problems they’re solving aren’t uniform.
- The following are the key factors from the twelve-factor app pattern methodology that play a role in enabling decentralized governance:
   
   - Dependencies – Decentralized governance allows teams to choose their own dependencies, so dependency isolation is critical to make this work properly.
   - Build, release, run – Decentralized governance should allow teams with diﬀerent build processes to use their own toolchains, yet should allow releasing and running the code to be seamless, even with diﬀering underlying build tools.
   - Backing services – If each consumed resource is treated as if it was a third-party service, then decentralized governance allows the microservice resources to be refactored or developed in diﬀerent ways, as long as they obey an external contract for communication with other services.

## Decentralized Data Management

- All data-bound communication should be enabled via services that encompass the data. As a result, each service team chooses the most optimal data store type and schema for their application.
- Decentralized data management enhances application design by allowing the best data store for the job to be used.

- The following are the key factors from the twelve-factor app pattern methodology that play a role in organizing around capabilities:
    - Disposability (maximize robustness with fast startup and graceful shutdown) – The services should be robust and not dependent on externalities. This principle further allows for the services to run in a limited capacity if one or more components fail.
    - Backing services (treat backing services as attached resources) – A backing service is any service that the app consumes over the network such as data stores, messaging systems, etc. Typically, backing services are managed by operations. The app should make no distinction between a local and an external service.
    - Admin processes (run admin/management tasks as one-oﬀ processes) – The processes required to do the app’s regular business, for example, running database migrations. Admin processes should be run in a similar manner, irrespective of environments.

- To achieve a microservices architecture with decoupled data management, the following software design patterns can be used:
  - Controller – Helps direct the request to the appropriate data store using the appropriate mechanism.
  - Proxy – Helps provide a surrogate or placeholder for another object to control access to it.
  - Visitor – Helps represent an operation to be performed on the elements of an object structure.
  - Interpreter – Helps map a service to data store semantics.
  - Observer – Helps deﬁne a one-to-many dependency between objects so that when one object changes state, all of its dependents are notiﬁed and updated automatically.
  - Decorator – Helps attach additional responsibilities to an object dynamically. Decorators provide a ﬂexible alternative to sub-classing for extending functionality.
  - Memento – Helps capture and externalize an object's internal state so that the object can be returned to this state later.
  
## Infrastructure Automation
- The following are the key factors from the twelve-factor app pattern methodology that play a role in evolutionary design:
  - Codebase (one codebase tracked in revision control, many deploys) – Because the infrastructure can be described as code, treat all code similarly and keep it in the service repository.
  - Conﬁg (store conﬁgurations in the environment) – The environment should hold and share its ow speciﬁcities.
  - Build, release, run (strictly separate build and run stages) – One environment for each purpose.
  - Disposability (maximize robustness with fast startup and graceful shutdown) – This factor transcends the process layer and bleeds into such downstream layers as containers, virtual machines, and virtual private cloud.
  - Dev/prod parity – Keep development, staging, and production as similar as possible.
  
- Ultimately, the goal is to enable developers to push code updates and have the updated application sent to multiple environments in minutes. 
- There are many ways to successfully deploy in phases, including the blue/green and canary methods. With the blue/green deployment, two environments live side by side, with one of them running a newer version of the application. 
- Traﬃc is sent to the older version until a switch happens that routes all traﬃc to the new environment.
- In this case, we use a switch of target groups behind a load balancer in order to redirect traﬃc from the old to the new resources. 
- Another way to achieve this is to use services fronted by two load balancers and operate the switch at the DNS level.

![image](https://user-images.githubusercontent.com/23625821/130197899-91a39937-5abb-4024-b795-fd0a434edfea.png)

## Design for Failure
- Everything fails all the time
- Here are the key factors from the twelve-factor app pattern methodology that play a role in designing for failure:
  - Disposability (maximize robustness with fast startup and graceful shutdown) – Produce lean container images and strive for processes that can start and stop in a matter of seconds.
  - Logs (treat logs as event streams) – If part of a system fails, troubleshooting is necessary. Ensure that material for forensics exists.
  - Dev/prod parity – Keep development, staging, and production as similar as possible.

- Modern container management services allow developers to retrieve near real-time, event-driven updates on the state of containers. Docker supports multiple logging drivers. 

![1](https://user-images.githubusercontent.com/23625821/130248108-20e52ba1-76e3-4fb1-a2e1-adcd57534097.png)

- Container monitoring solutions use metric capture, analytics, transaction tracing and visualization.
- Container monitoring covers basic metrics like memory utilization, CPU usage, CPU limit and memory limit. 
- Container monitoring also oﬀers the real-time streaming logs, tracing and observability that containers need.

## Evolutionary Design
- The following are the key factors from the twelve-factor app pattern methodology that play a role in evolutionary design:
  • Codebase (one codebase tracked in revision control, many deploys) – Helps evolve features faster since new feedback can be quickly incorporated.
  • Dependencies (explicitly declare and isolate dependencies) – Enables quick iterations of the design since features are tightly coupled with externalities.
  • Conﬁguration (store conﬁgurations in the environment) – Everything that is likely to vary between deploys (staging, production, developer environments, etc.). Conﬁg varies substantially across deploys, code does not. With conﬁgurations stored outside code, the design can evolve irrespective of the environment.
  • Build, release, run (strictly separate build and run stages) – Help roll out new features using various deployment techniques. Each release has a speciﬁc ID and can be used to gain design eﬃciency and user feedback.

- The following software design patterns can be used to achieve an evolutionary design:
  • Sidecar extends and enhances the main service.
  • Ambassador creates helper services that send network requests on behalf of a consumer service or application.
  • Chain provides a deﬁned order of starting and stopping containers.
  • Proxy provides a surrogate or placeholder for another object to control access to it.
  • Strategy deﬁnes a family of algorithms, encapsulates each one, and makes them interchangeable. Strategy lets the algorithm vary independently from the clients that use it.
  • Iterator provides a way to access the elements of an aggregate object sequentially without exposing its underlying representation.
  • Service Mesh is a dedicated infrastructure layer for facilitating service-to-service communications between microservices, using a proxy. 

- Deployment strategies such as a Canary release provide added agility to evolve design based on user feedback. 
- Canary release is a technique that’s used to reduce the risk inherent in a new software version release. 
- In a canary release, the new software is slowly rolled out to a small subset of users before it’s rolled out to the entire infrastructure and made available to everybody. 

- In the diagram that follows, a canary release can easily be implemented with containers using AWS primitives. 
- As a container announces its health via a health check API, the canary directs more traﬃc to it. 
- The state of the canary and the execution is maintained -using Amazon DynamoDB, Amazon Route 53 , Amazon CloudWatch, Amazon Elastic Container Service (Amazon ECS), and AWS Step Functions.

![1](https://user-images.githubusercontent.com/23625821/130248884-5e1cda6e-134c-4871-aec7-83f00a90c31e.png)

## Conclusion
- Microservices can be designed using the twelve-factor app pattern methodology and software design patterns enable you to achieve this easily. These software design patterns are well known. 

- If applied in the right context, they can enable the design beneﬁts of microservices. AWS provides a wide range of primitives that can be used to enable containerized microservices.


### References 

<a href="https://docs.aws.amazon.com/whitepapers/latest/running-containerized-microservices/running-containerized-microservices.pdf#welcome"> Original paper </a>
