# Lection 2("Database design")

## Introduction

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; _**Database modeling(design)**_ is the process of defining the structure, relationships, and constraints that will be applied to the database to be created.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Modern database modeling workflow consisnts of:

- _**Conceptual Stage:**_ Focus entirely on business rules. Identify entities (e.g., User, Order) and how they interact. You do not need to choose a database type yet.
- _**The Pivot Point (The Transition):**_ Evaluate your application's data volume, scaling needs, and structure variety. Make the RDBMS vs. NoSQL choice right here.
- _**Logical Stage:**_ Apply the rules of your chosen DBMS type. Create clean, normalized relational tables OR design nested, query-optimized NoSQL document schemas.
- _**Physical Stage:**_ Translate your logical structure into actual code for a specific engine, like PostgreSQL or MongoDB.

_**Requirements gathering** is done at the conceptual stage. But it would not be a mistake to place requirements gathering as a separate item before the conceptual stage._

_**!** Compared to the classic database design process, a stage of choosing the DBMS type has been added. The old process was envisaged when the default standard was an RDBMS with a relational data model (the context of the database architecture), and the issue of choosing the DBMS type was not relevant. Today, NoSQL DBMSs are a worthy competitor to RDBMSs, and the stage of creating their logical data model(design context) is different._

| Feature                     | SQL (Relational)                                                                                  | NoSQL (Non-Relational)                                                              |
| :-------------------------- | ------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------- |
| **Core Structure**          | Normalized tables with rows and columns.                                                          | Collections of documents (JSON/BSON), key-value pairs, or graphs.                   |
| **Schema Design**           | Rigid and predefined. Schema must be defined before inserting data.                               | Flexible and dynamic. Fields can vary from one record to the next.                  |
| **Handling Relationships**  | Uses Foreign Keys and junction tables to link separate tables (normalization).                    | Uses Embedding (nested documents) or Referencing (storing IDs) to handle relations. |
| **Data Duplication**        | Minimized. Data is normalized to avoid storing the same information twice.                        | Often denormalized. Duplicating data is common to speed up reads.                   |
| **Flexibility for Changes** | Harder to change. Altering a table requires database migrations and downtime or careful planning. | Easier to change. Adding a new field to one document does not break others.         |
| **Primary Design Goal**     | Data integrity and reducing redundancy via ACID compliance.                                       | Access patterns and query speed for specific application needs.                     |

## Database design in practice

### Bussiness requirements

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;A car rental company owner wants to improve the monitoring of their company's processes. They want to be able to see how many cars each branch rents, evaluate the performance of their employees, track the pick-up and return of rental cars, and see which cars are most popular at which branch.

**Domain area description:**

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;A customer rents cars from branches located in different cities. Each car is associated with one branch. Car rental operations are carried out by a specific employee. Each employee can work in only one branch. The owner wants to have the entire rental history.

### Conceptual stage

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;_**Conceptual data model**_ is a high-level,technology-independent representation of organizational data objects, core business concepts, and their relationships. Pioneered in database architecture by Peter Chen, it defines the "what" of a system layout without physical storage constraints.

Steps required to build a conceptual model:

#### Prioritize Thorough Requirements Gathering First

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Before drawing diagrams, engage heavily with business stakeholders to understand their workflows, problems, and expected outcomes.

- Action: Interview domain experts, analyze existing paperwork or legacy systems, and draft clear user stories. Don't start designing until the business logic is fully mapped out

#### Identify Entities, Not Tables

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;An entity represents a real-world person, place, object, or event relevant to the business.

- Action: Keep entities abstract. For example, identify an entity as Customer or Product. Do not worry about SQL tables, schemas, or foreign keys at this stage.

#### Establish High-Level Relationships and Cardinality

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Define how your entities interact with each other using real-world business rules.Try to avoid entities that has more than one purpose.

Action: Identify relationships using verbs (e.g., A Customer places an Order). Explicitly document the business cardinality (e.g., "Can a customer place multiple orders?" or "Can an order belong to multiple customers?").

#### Model Only Core Attributes

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Avoid the temptation to map out every single data field or specific data type (like VARCHAR(255) or INT).

- Action: Only include critical, descriptive business attributes that define the entity (e.g., a Customer has a Name and an Email). Leave the technical keys, indexes, and nullability constraints for the later logical and physical stages.

#### Leverage Visual Entity-Relationship (ER) Modeling

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Human brains process visuals much faster than text descriptions. Use a standardized modeling notation (like Chen's Notation or Crow's Foot Notation) to create a high-level Entity-Relationship Diagram (ERD).

- Action: Keep the visualization clean, simple, and extensible. A non-technical stakeholder should be able to look at your conceptual ERD and instantly tell you if the business logic is correct.

#### Keep it Fully Platform-Agnostic

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;A great conceptual model looks exactly the same whether you plan to implement it in a relational SQL database, a NoSQL document store, or a graph database.

- Action: Actively resist using database-specific jargon (e.g., avoid "surrogate keys", "indexes", or "sharding") in your conceptual documentation.

#### Conceptual schema for car rent company

![alt text](images\CarRentalConceptual.svg)

#### Conceptual data model (schema) conclusion

**_ModelFocus:_** The "what" of the business (core concepts and big-picture rules). \
**_Details:_** High-level entities (like Customer, Product, Order) and their general connections.\
**_Technology:_**: 100% independent of any software or database.\
**_Audience:_** Business leaders and stakeholders.

### The Pivot Point (The Transition)

We choose RDBMS with relational data model (database theory context)

### Logical data model stage

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;A _**logical data model**_ is a detailed, vendor-agnostic blueprint that structures data elements and their operational boundaries based on a pre-selected database paradigm (Relational or NoSQL).While it remains independent of specific software vendors (like Oracle vs. PostgreSQL, or MongoDB vs. DynamoDB), it is deeply dependent on the data architecture paradigm chosen to solve the business problem. It acts as a bridge between high-level business concepts and technical implementation.
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; In case of RDBMS at completed logical data model(database design context) with:

- Tables(Entities): Represent distinct real-world objects, places, or concepts (such as Customer or Order).
- Attributes: Define the specific properties or characteristics of an entity (such as Customer Name or Email Address).
- Primary Keys: Serve as unique identifiers for each specific record within an entity.
- Foreign Keys: Establish links between separate entities by referencing a primary key in a related table.
- Relationships: Define how entities associate with one another, including cardinality (one-to-one, one-to-many, or many-to-many) and optionality.
- Junction Tables: Resolve many-to-many relationships into manageable one-to-many connections

Steps to get logical model from conceptual:

![alt text](images\FromConceptualToLogical+normalizw.JPG)

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;For logical model creation we will use ERD (Crow's Foot Notation (Information Engineering / IE)) to vizualize the relationship between entities within a database. It is a design or blueprint of the database.\

![alt text](images\logicalCarRent.svg)

#### Logical data model (schema) conclusion

**_Logical Data ModelFocus:_** The structural blueprint (adds detail and organization).\
**_Details:_** Specific attributes, primary keys, foreign keys, and normalization rules (e.g., customer id, email).\
**_Technology:_** Still independent of any specific database engine (Don't mess with DBMS type definition. At this stage, the type of DBMS that will be used is determined, but the specific version of the DBMS remains an abstraction. For example, we make desicion to use a relational DBMS, but we do not determine which relational DBMS will be used. Whether it is PostgreSQL, either MS SQL, either My SQL e.t.c. ).\
**_Audience:_** Data analysts and system architects.

### Phisycal data model (schema)

A physical data model is a database-specific blueprint that shows how data is structured, stored, and managed within a particular Database Management System (DBMS)
