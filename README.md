
# More Practice With SQL Queries - Lab

## Introduction

In this lesson, we'll run through some practice questions to refresh our knowledge of SQL Queries!

## Objectives

You will be able to:

- Practice your SQL knowledge

## Getting Started

As in previous labs, we'll make use of the `sqlite3` library as well as `pandas`. By combining them, we'll be able to write our queries as python strings, and make sure that the results are always returned as a pandas DataFrame. 

We'll start by loading both libraries and connecting to the database we'll be using for this lab, `data.sqlite`. You may remember this database from a previous lab. As a refresher, here's the ERD diagram for this database: 

<img src='images/Database-Schema.png'>

In the cell below:

* Import the necessary libraries `pandas` and `sqlite3`
* Establish a connection to the database `data.sqlite`
* Get the `cursor` from the connection and store it in the variable `c`.


```python
import sqlite3
import pandas as pd
conn = sqlite3.connect('data.sqlite')
c = conn.cursor()
```

## Basic Queries

Now, let's review basic SQL queries. In the cell below:

* Write a query that gets the first name, last name, phone number, address, and credit limit for all customers in California with a credit limit greater than 25000.00. 


```python
# For the first query, the boilerplate for getting 
#the query into a dataframe has been provided for you
c.execute("""SELECT contactFirstName, contactLastName, phone, addressLine1, state, creditLimit
from customers WHERE creditLimit > 25000 AND state = 'CA'""")
df = pd.DataFrame(c.fetchall())
df.columns = [x[0] for x in c.description]
df.head(10)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>contactFirstName</th>
      <th>contactLastName</th>
      <th>phone</th>
      <th>addressLine1</th>
      <th>state</th>
      <th>creditLimit</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Susan</td>
      <td>Nelson</td>
      <td>4155551450</td>
      <td>5677 Strong St.</td>
      <td>CA</td>
      <td>210500.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Julie</td>
      <td>Murphy</td>
      <td>6505555787</td>
      <td>5557 North Pendale Street</td>
      <td>CA</td>
      <td>64600.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Juri</td>
      <td>Hashimoto</td>
      <td>6505556809</td>
      <td>9408 Furth Circle</td>
      <td>CA</td>
      <td>84600.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Julie</td>
      <td>Young</td>
      <td>6265557265</td>
      <td>78934 Hillside Dr.</td>
      <td>CA</td>
      <td>90700.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Valarie</td>
      <td>Thompson</td>
      <td>7605558146</td>
      <td>361 Furth Circle</td>
      <td>CA</td>
      <td>105000.0</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Julie</td>
      <td>Brown</td>
      <td>6505551386</td>
      <td>7734 Strong St.</td>
      <td>CA</td>
      <td>105000.0</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Brian</td>
      <td>Chandler</td>
      <td>2155554369</td>
      <td>6047 Douglas Av.</td>
      <td>CA</td>
      <td>57700.0</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Sue</td>
      <td>Frick</td>
      <td>4085553659</td>
      <td>3086 Ingle Ln.</td>
      <td>CA</td>
      <td>77600.0</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Steve</td>
      <td>Thompson</td>
      <td>3105553722</td>
      <td>3675 Furth Circle</td>
      <td>CA</td>
      <td>55400.0</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Sue</td>
      <td>Taylor</td>
      <td>4155554312</td>
      <td>2793 Furth Circle</td>
      <td>CA</td>
      <td>60300.0</td>
    </tr>
  </tbody>
</table>
</div>



#### Expected Output

<img src='images/expected-output-1.png'>

## Aggregate Functions and GROUP BY

Next, write a query that get sthe average credit limit per state.


```python
  c.execute("""SELECT state, AVG(creditLimit)
from customers GROUP BY state""")
df = pd.DataFrame(c.fetchall())
df.columns = [x[0] for x in c.description]
df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>state</th>
      <th>AVG(creditLimit)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td></td>
      <td>61839.726027</td>
    </tr>
    <tr>
      <th>1</th>
      <td>BC</td>
      <td>89950.000000</td>
    </tr>
    <tr>
      <th>2</th>
      <td>CA</td>
      <td>83854.545455</td>
    </tr>
    <tr>
      <th>3</th>
      <td>CT</td>
      <td>57350.000000</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Co. Cork</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Isle of Wight</td>
      <td>93900.000000</td>
    </tr>
    <tr>
      <th>6</th>
      <td>MA</td>
      <td>70755.555556</td>
    </tr>
    <tr>
      <th>7</th>
      <td>NH</td>
      <td>114200.000000</td>
    </tr>
    <tr>
      <th>8</th>
      <td>NJ</td>
      <td>43000.000000</td>
    </tr>
    <tr>
      <th>9</th>
      <td>NSW</td>
      <td>100550.000000</td>
    </tr>
    <tr>
      <th>10</th>
      <td>NV</td>
      <td>71800.000000</td>
    </tr>
    <tr>
      <th>11</th>
      <td>NY</td>
      <td>89966.666667</td>
    </tr>
    <tr>
      <th>12</th>
      <td>Osaka</td>
      <td>81200.000000</td>
    </tr>
    <tr>
      <th>13</th>
      <td>PA</td>
      <td>84766.666667</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Pretoria</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>15</th>
      <td>Queensland</td>
      <td>51600.000000</td>
    </tr>
    <tr>
      <th>16</th>
      <td>Québec</td>
      <td>48700.000000</td>
    </tr>
    <tr>
      <th>17</th>
      <td>Tokyo</td>
      <td>94400.000000</td>
    </tr>
    <tr>
      <th>18</th>
      <td>Victoria</td>
      <td>88800.000000</td>
    </tr>
  </tbody>
</table>
</div>



#### Expected Output

<img src='images/expected-output-2.png'>

## JOINs

Now, write a query that uses JOIN statements to get the customer name, customer number, order number, status, and quantity ordered. Print only the head of this DataFrame. 


```python
c.execute("""SELECT contactFirstName, contactLastName, customers.customerNumber, 
                    orders.orderNumber, status, SUM(quantityOrdered)
FROM customers JOIN orders ON customers.customerNumber = orders.customerNumber 
JOIN orderdetails ON orders.orderNumber = orderdetails.orderNumber
GROUP BY 1, 2, 3, 4, 5""")
df = pd.DataFrame(c.fetchall())
df.columns = [x[0] for x in c.description]
df.head(10)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>contactFirstName</th>
      <th>contactLastName</th>
      <th>customerNumber</th>
      <th>orderNumber</th>
      <th>status</th>
      <th>SUM(quantityOrdered)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Adrian</td>
      <td>Huxley</td>
      <td>282</td>
      <td>10139</td>
      <td>Shipped</td>
      <td>266</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Adrian</td>
      <td>Huxley</td>
      <td>282</td>
      <td>10270</td>
      <td>Shipped</td>
      <td>374</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Adrian</td>
      <td>Huxley</td>
      <td>282</td>
      <td>10361</td>
      <td>Shipped</td>
      <td>429</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Adrian</td>
      <td>Huxley</td>
      <td>282</td>
      <td>10420</td>
      <td>In Process</td>
      <td>532</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Akiko</td>
      <td>Shimamura</td>
      <td>398</td>
      <td>10258</td>
      <td>Shipped</td>
      <td>200</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Akiko</td>
      <td>Shimamura</td>
      <td>398</td>
      <td>10339</td>
      <td>Shipped</td>
      <td>614</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Akiko</td>
      <td>Shimamura</td>
      <td>398</td>
      <td>10372</td>
      <td>Shipped</td>
      <td>321</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Akiko</td>
      <td>Shimamura</td>
      <td>398</td>
      <td>10408</td>
      <td>Shipped</td>
      <td>15</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Allen</td>
      <td>Nelson</td>
      <td>379</td>
      <td>10147</td>
      <td>Shipped</td>
      <td>341</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Allen</td>
      <td>Nelson</td>
      <td>379</td>
      <td>10274</td>
      <td>Shipped</td>
      <td>161</td>
    </tr>
  </tbody>
</table>
</div>



#### Expected Output

<img src='images/joins.png'>

## HAVING and ORDER BY

Now, return the customerName, customrerNumber, productName, productCode and total number ordered for any product a customer has bought 10 or more of cumulatively. Sort the rows in descending order by the quantity ordered. 

**_Hint_**: For this one, you'll need to make use of HAVING, GROUP BY, and ORDER BY--make sure you get the order of them correct!


```python

```

#### Expected Output

<img src='images/having_order.png'>

## Subqueries

Finally, get the first name, last name, employee number, and office code for employees from an office with less than 5 employees. 


```python

```

#### Expected Output

<img src='images/expected-output-5.png'>

# Summary

In this lesson, we reviewed all the major concepts and keywords associated with SQL queries!
