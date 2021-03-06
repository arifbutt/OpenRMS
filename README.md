# OpenRMS

~ This document is a work in progress ~

Open RMS is an Open Source project with the intention of delivering a retail management system that is free to install, free to use, free to modify and free to distribute. The aim of the product is to provide system which has key modules which can then be extended with private bespoke modules built by developers within retail businessed or additional public modules developed by the wider open source community for the expansion of the project. The product will continue to evolve as the key technologies mature. 

## Target Consumers

The finished product will be aimed primarily at small to medium size business where Microsoft Excel and Access based solutions are far from ideal, and Microsoft and Oracle solutions are till to expensive or awkward to implement for the current maturity of the business.

## Wiki
The wiki for this project is available from the Wiki section of this GitHub repository, or can be cloned locally The suggested local folder location for the Wiki is the Wiki sub-folder of main solution. 

## Development
Open RMS will be developed on a .Net platform using .Net Core version 2.0 and will be developed within a Visual Studio 2017+ solution. It will feature key modules like *Product Managment*, *Product Metadata*, *Product Stock*, *Location Management* (both *Store* and *Distribution Centre / Hubs*) as part of the main open source code but will allow other modules to be developed independently and plugged into the main application.

The application will be able to be hosted on public internet servers or private *on-prem* or *off-prem* intranet servers.

### Development Environment
- Visual Studio 2017 
- StyleCop (To set agreed styling rules)
- CodeMade (optional - to apply StyleCop rules) 

#### Stylecop Rule and Exclusions
Infomarmation on StyleCop rules and exclusions can be found [here](https://github.com/dibley1973/OpenRMS/wiki/StyleCop-Rules-and-Agreed-Exclusions)

### Milestones
The development milestones can be found [here](https://github.com/dibley1973/OpenRMS/milestones). There will be an ongoing process to ensure they are up to date. 


### Modules
The application will be split into modules which in most cases will map directly to a "bounded context" within the the problem domain. Although the application will share a single database, each module will have it's own stack from client to data access and databse project. This will allow multiple modules to be worked upon at any one time with limited impact to the other modules. There will be common elements that will be shared accross all modules, like for instance the `SharedKernel` which might contain the base classes for Entities and ValueObjects.

Below is a list of initial modules but the intension would be to make the application extensible so new modules could be written and plugged in as the requirement becomes appparent.

+ Location Management (2nd Milestone)
+ Product Managment (3rd Milestone)
+ Product Location Management
+ Location Attribute Management
+ Product Attributes  Management
+ Product Inventory
+ Store Metadata
+ Distribution Managment
+ Product in Transit
+ Supply Chain Management

#### Location Management
The Location Managent module will be developed first and once the code, sturcture and format is complete and "signed off" then this will be the template module which all other modules should adhere to. 

#### Product Management

### Architecture, Patterns, Technologies and Methodologies
It is intended that the solution will use the following architecture, patterns, technologies and methodologies.

+ Architecture
 + CQRS (Command Query Responsability Segragation) 
 + Onion / Hexagonal Architecture
+ Patterns and Practices
 + Domain Driven Design 
 + Dependecy Injection
 + Test First or Test After Test driven development
 + Repository pattern
 + Unit of Work pattern
 + CQS (Command Query Separation)
+ Technologies
 + MVC (to serve master views)
 + Web API (for CRUD)
 + Angular 2
 + Sql Server 2016
+ Methodologies

#### Architecture

+ Type 1 CQRS (Command Query Responsability Segragation) 
 + Queries
 + Commands
+ Onion / Hexagonal Architecture

##### Type 1 CQRS (Command Query Responsability Segregation) 
The project will use Type 1 CQRS so will have the same "stack" for commands and queries. [EntityFramework](https://www.nuget.org/packages/EntityFramework/) for the *Command Stack* and the *Query Stack*.

###### Queries
WHere a query isexpected to return zero or one result the object returned should either be wrapped in a `Maybe<T>` or return a valid instance or an object following a null-object pattern. Never shoud a result return a NULL value.

Where a query is to returns zero or more results then it should return an IEnumerable<T> with zero or more elements. It shouldnever return a NULL value. Where arguments are more than one or two in length an object should be defined to hold the parameters. If paging is required then the object should implement the `IPagedQueryParameter` interface which contains the `StartAt` property which indicates where the paging is to start and the `Take` property indicating upto how many records to take. The 

###### Commands
The *Command Stack* will use Onion architecture and a Domain Driven Development practice. Commands should never return a value and should always be defined as a `void` method. Identities for new entities should be created by the domain and assigned to the Entities before inserting into the database. IF the identity for the entity is a *Long Integer* then the High-Low principle may be followed with the next "n" available identities queried from the database. If the identity for the entity is an Guid then the domain can generate it's own.

A simple example of the intended CQRS architecture can be found at this blog post [Using Entity Framework and the Store Procedure Framework To Achieve CQRS](http://www.duanewingett.info/2016/08/02/UsingEntityFrameworkAndTheStoreProcedureFrameworkToAchieveCQRSPart1.aspx)

 

##### Onion / Hexagonal Architecture
At the centre of the onion will be the *Shared.Kernel* which will contain the basic building blocks needed and share accross all bounded contexts. This is where the base `Entity` and `ValueObject` classes will reside. It will also contain any *non*-problem domain objects, for instance the `Maybe<T>` amplifier. Around the *Shared.Kernel* will be wrapped the *Domain.Core*. This is where all of the domain logic should live within the *Domain Entities*, *Value Objects*, *Domain Events* and *Aggregates*. Around this layer will be wrapped the *Domain Services*, *Factories* and *Repositories*. Around this layer will wrap the *Application Services* and the *Integration Tests*.

![Onion Architecture](https://github.com/dibley1973/OpenRMS/blob/master/Documentation/Images/Readme_OnionArchitecture.png?raw=true "Onion Architecture")

#### Patterns and Practices
##### Domain Driven Design 
##### Dependecy Injection
##### Test First or Test After Test driven development
##### Repository pattern
##### Unit of Work pattern
##### CQS (Command Query Separation)
The will be a string drive to ensure a separation of commands and queries in methods. To try and 

#### Technologies
##### MVC (to serve master views)
##### Web API
##### TypeScript
##### Angular 2


### Folder Structure
TBC


### Namespacing
Even though abreviations in code are evil the base namespace will be `ORMS`, standing for "Open Retail Management Service".

Examples of the name spacing for the *Shared Kernel* and the *Product Management* module are shown below.

```
ORMS
ORMS.Shared.SharedKernel
ORMS.Shared.SharedKernel.UnitTests
ORMS.ProductManagement.ApplicationServices
ORMS.ProductManagement.ApplicationServices.IntegrationTests
ORMS.ProductManagement.Domain.Services
ORMS.ProductManagement.Domain.Entities
ORMS.ProductManagement.Domain.Repositories
```

### Automated Tests
Both unit tests and integration tests will be applied to cover as much code as sensibly possible. 100% test coverage is not possible for the whole project, but should be achevable for **ALL** code in the Domain.

### Documentation
+ Format
+ Location

#### Format
Where appropriate project documentation should be in markdown with a suffix of ".md"

#### Location
Where possible project documentation should be in appropriately nested folders with the "Documentation" folder inside the solution root. The exception to this is the "README.md" and "LICENCE" files which should remain at the root.

### Contribution

Anyone is welcome to contribute. You do not need to have retail software experience but it will help. You do not need to be a full stack developer. If you have a strength in HTML and CSS feel free to join. If your preference is in database work, again, you are more than welcome. The project will need a community with a wide skill set yet with deep knowledge of the technologies, patterns and practices. If you can give 30 minutes a day, 30 minutes a week, month or year, you are still as welcome as an equal project member. (I certainly don't have all of the skills or time to carry out this project on my own! - *dibley1973*)
