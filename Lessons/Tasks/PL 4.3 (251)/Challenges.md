
PL 2.5 tasks

1. Create view that returns most pupular films ( by payments quantity) for all categories

2. Create function that  returns day of week in format 'Понеділок'. 'Вівторок' ... e.t.c. from input datetime parameter 

3. Create stored procedure that accepts actor _**first name, last name**_ and returns information about all films the actor is starred in. Result format: _**film name, film category, film payments count, film payments total amount**_. Also into output parameter  _**payment_by_actor**_ write total payments sum for all the films actor is starred in.

4. Create trigger for _**transfers**_ table
CREATE TABLE transfers (
    id          INT PRIMARY KEY AUTO_INCREMENT,
    from_id     INT,
    to_id       INT,
    amount      DECIMAL(12,2),
    transferred_at DATETIME DEFAULT CURRENT_TIMESTAMP
);
to add  after each insert operation data into _**tax_control**_ table ( tax_amount should be 20 % from transfer amount)

CREATE TABLE tax_control (
    id          INT PRIMARY KEY AUTO_INCREMENT,
    transfer_id    INT,
    tax_amount      DECIMAL(12,2),
    transferred_at DATETIME
);
