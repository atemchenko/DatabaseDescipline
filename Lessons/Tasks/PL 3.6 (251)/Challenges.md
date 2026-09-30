
## Challenges for practical lesson 3.6
## Question 1:

#### Level: 
Simple

#### Topic: 
DISTINCT

#### Task: 
Create a list of all the different (distinct) replacement costs of the films.

_**Question:**_ 
What's the lowest replacement cost?


_*lowest replacement cost*_ - **9.99**

## Question 2:

#### Level: 
Simple

#### Topic: 
LIKE

#### Task: 
Get all information about actors who have the letter "r" in the third position of their name. Sort the result by last name from Z to A.

_**Question:**_ 
How many actors with the letter "r" in the third position of their name?

_*How many actors with the letter "r" in the third position of their name?*_ - **34**

## Question 3:

#### Level: 
Simple

#### Topic: 
CAST

#### Task: 
Get payment date and amount for payments  where the amount is either 0
or is between 3.99 and 7.99 and has
happened on 2020-01-25.

**_Question:_**
How many payments where the amount is 0 or between 3.99 and 7.99 were made on 01/25/2020?

_*There are **67** payments meet the conditions.*_

## Question 4:

#### Level: 
Simple

#### Topic: 
group by, dayofweek

#### Task: 
Show total payment amount by day of week? (1=Sunday, 2=Monday, 3=Tuesday, 4=Wednesday, 5=Thursday, 6=Friday, 7=Saturday)

**_question:_**
What's the day of week with the highest total payment amount?

_*What's the day of week with the highest total payment amount?*_ - **_Thursday_**

## Question 5:

#### Level: 
Moderate

#### Topic: 
CASE + GROUP BY

#### Task: 
Write a query that gives an overview of how many films have replacements costs in the following cost ranges

low: 9.99 - 19.99

medium: 20.00 - 24.99

high: 25.00 - 29.99

_**Question:**_ How many films have a replacement cost in the "low" group?

_There are **514** films in low group_

## Question 6:

**Level:** Moderate

**Topic:** JOIN

**Task:** Create a list of the film titles including their title, length, and category name ordered descendingly by length. Filter the results to only the movies in the category 'Drama' or 'Sports'.

**Question:** In which category is the longest film and how long is it?

_In which category is the longest film and how long is it?_ - **Sports and 184**

## Question 7:

**Level:** Moderate

**Topic:** JOIN & GROUP BY

**Task:** Create an overview of how many movies (titles) there are in each category (name).

**Question:** Which category (name) is the most common among the films?

_Which category (name) is the most common among the films?_ - Sports with 74 titles

## Question 8:

**Level:** Moderate

**Topic:** JOIN & GROUP BY

**Task:** Create an overview of the actors' first and last names and in how many movies they appear in.

**Question:** Which actor is part of most movies??

_Which actor is part of most movies?_ - **Susan Davis with 54 movies**

## Question 9:

**Level:** Moderate

**Topic:** LEFT JOIN & FILTERING

**Task:** Create an overview of the addresses that are not associated to any customer.

**Question:** How many addresses are that?

_ How many addresses are that?_ - **4**

## Question 10:

**Level:** Moderate

**Topic:** JOIN & GROUP BY

**Task:** Create the overview of the sales  to determine the from which city (we are interested in the city in which the customer lives, not where the store is) most sales occur.

**Question:** What city is that and how much is the amount?

_What city is that and how much is the amount?_ - **Cape Coral with a total amount of 221.55**

## Question 11:

**Level:** Moderate to difficult

**Topic:** JOIN & GROUP BY

**Task:** Create an overview of the revenue (sum of amount) grouped by a column in the format "country, city".

**Question:** Which country, city has the least sales?

_Which country, city has the least sales?_ - **United States, Tallahassee with a total amount of 50.85.**

## Question 12:

**Level:** Difficult

**Topic:** Uncorrelated subquery

**Task:** Create a list with the average of the sales amount each staff_id has per customer.

**Question:** Which staff_id makes on average more revenue per customer?

_Which staff_id makes on average more revenue per customer?_- **staff_id 2 with an average revenue of 56.64 per customer.**

## Question 13:

**Level:** Difficult 

**Topic:** date, dayofweek, group by + Uncorrelated subquery

**Task:** Create a query that shows average daily revenue of all Sundays.

**Question:** What is the daily average revenue of all Sundays?

_What is the daily average revenue of all Sundays?_ - **1428.60**


## Question 14:

**Level:** Difficult to very difficult

**Topic:** Correlated subquery

**Task:** Create a list of movies - with their length and their replacement cost - that are longer than the average length in each replacement cost group.

**Question:** Which two movies are the shortest on that list and how long are they?

_Which two movies are the shortest on that list and how long are they?_ - **CELEBRITY HORN and SEATTLE EXPECTATIONS with 110 minutes.**


## Question 15:

**Level:** Very difficult

**Topic:** Uncorrelated subquery

**Task:** Create a list that shows the "average customer lifetime value" grouped by the different districts.

**Example:**
If there are two customers in "District 1" where one customer has a total (lifetime) spent of $1000 and the second customer has a total spent of $2000 then the "average customer lifetime spent" in this district is $1500.

So, first, you need to calculate the total per customer and then the average of these totals per district.

**Question:** Which district has the highest average customer lifetime value?

_Which district has the highest average customer lifetime value?_ - **Saint-Denis with an average customer lifetime value of 216.54.**

