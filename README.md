The project has been developed using .NET 7, demonstrating a fusion of monolithic architecture principles with microservices methodologies. It follows a structured approach, segmenting the project into distinct domains to achieve a well-organized and scalable system.

The example undertaking comprises of both users and their corresponding tasks.
A user is described by an id, a status, as well as their first and last name.
A user's task is outlined by an id, a status, a title, a description, an author (the user), and potentially an assigned user identification number.

# Project structure
The project has been structured into different domains:
- [**Api**](#Api)
- [**Components**](#Component)
- [**DataModel** ](#DataModel)
- [**Events** ](#Events)
- [**EventHandlers** ](#EventHandlers)
- [**Infrastructure** ](#Infrastructure)
- [**BackgroundWorker**](#BackgroundWorker)

Overall, the project exemplifies a well-considered architectural approach that combines the advantages of both monolithic and microservices paradigms, fostering modularity, maintainability, and scalability.

The project leverages essential external dependencies:

- **Ulid**: This library provides unique identifiers that serve as primary keys in the database, contributing to data integrity and accuracy.
- **Mediatr**: As an in-process messaging framework, Mediatr streamlines communication between components, minimizing dependencies and enhancing efficiency.
- **FluentValidation**: By ensuring proper validation of incoming requests, FluentValidation contributes to the reliability of the application's data handling.
- **Hangfire**: Permits the application to execute background processing essential for event management.

## Generating strongly typed ids
A distinctive aspect of the project is the use of strongly typed IDs, which are employed as unique identifiers throughout the database. These IDs are prefixed with the corresponding domain abbreviation, mitigating potential errors when dealing with foreign keys and database joins—drawing inspiration from the concept of Stripes object IDs.

## Api
Serving as the primary interface for the application, the API is built using .NET and is responsible for routing incoming requests. This involves directing requests to their respective handlers within the "Components" section. Additionally, the "Api" domain encompasses the creation of Data Transfer Objects (DTOs) that bridge the gap between the API and the components, along with filters like action and authorization filters, if needed.

It consists of the following:
- Controllers: Exposing api routes and directing routes to the different handlers in components
- Dtos: If some DTOs differs from the requests/responses in components
- Filters:  In case filters are needed (e.g. action-, authorization-filters, etc.)

## Components
At the heart of the application's business logic, the "Components" domain handles all CRUD (Create, Read, Update, Delete) operations. This domain is subdivided into queries (for read operations) and commands (for write operations), adhering to the principles of Command Query Responsibility Segregation (CQRS).

## DataModel
Within this section, the application's data structures and relationships are defined. This underpins the database schema and the models that interact with the data.

## Events
Events, encapsulated as event models or objects, facilitate communication across the application's various components. By utilizing an "IEvent" interface, events are decoupled from specific components and can trigger actions more flexibly.

## EventHandlers
Event handlers play a crucial role in responding to events propagated within the application. Through the use of an "IEventHandler" interface, these handlers execute specific actions in response to events, fostering loose coupling between different parts of the system.

## Infrastructure
The "Infrastructure" section addresses cross-cutting concerns and setup tasks. This includes implementing authentication, Cross-Origin Resource Sharing (CORS), middleware, and more. Notably, the infrastructure layer incorporates the concepts of CQRS, employing a mediator, command, query, and event bus pattern to facilitate communication and interaction between components.

Lastly, the inclusion of Swagger for API documentation enhances the project's usability and developer experience by providing clear insights into the API's structure and endpoints.

## BackgroundWorker
A background worker serves the purpose of event management. Events are employed to manage side effects, and consequently, we aim to prevent events from obstructing endpoint execution. In this project, Hangfire is employed, and to reduce the workload on the API instance, it has been relocated to a separate instance. This can be effortlessly accomplished by leveraging the infrastructure to configure its dependencies.  
