# Task for practical lesson 1.6 (241,242,243)
## Result expectations
- Analyze the requirements and create a conceptual data model ERD (Peter Chen notaton) that reflects the application area for which you need to create a database. 
- Create a logical data model (ERD) using "Crow's Foot" (Information Engineering - IE) notation for a relational database.
    - The logical data model must comply with Third Normal Form (3NF) requirements.
    - Primary keys (PK) and foreign keys (FK) must be defined.
    - Entities must be linked according to the established relationship multiplicity (cardinality).

    - [logical data model ERD  example](<../../../../../for cadets/Lessons/Database design(Lection 1.4)/ERD_Crow's Foot Notation_Information Engineering  IE.drawio>)

**! _Relational database design process is described in Lessons\Database design_([Lection 1.4](https://github.com/atemchenko/DatabaseDescipline/tree/main/Lessons/Database%20design(Lection%201.4)))**

## Requirements

Pizza shop owner asked to provide application will he owner of three local pizza shops, to set up a simple database so that
he can use to run his business. He essentially wants to track customer pizza orders to ensure that his staff completes and delivers the pizza.

- Customers order pizzas and either pick them up or have them delivered.

- Orders are handled by employee. Employee works in only one shop.

- An order may contain different quantities of different products.

- The shop take orders for pizzas.

- The owner owns several pizza shops.

- Owner wants to track sales by store, date, time and coupon applied.

- Pizzas are either delivered or picked up at the store.

Currently, the pizzeria owner tracks orders in a spreadsheet.

![alt text](Spreadsheet-1.jpg)