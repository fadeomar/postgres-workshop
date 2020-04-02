#### 1. Find the first name and surname of every author

solution : `select first_name, surname from authors;`

#### 2. Sort everyone by surname and find the first three

to order the results in ascending order: 
  `select * from authors order by first_name;`

to order the results in descending order: 
  `select * from authors order by first_name DESC;`

to select a spacific number of rows in sql : 
  `select * from authors limit 3;`

solution: `SELECT * FROM authors ORDER BY surname LIMIT 3;`

#### 3. Find everyone who has a location specified

best way to return any column doesn’t contain NULL values is to use the IS NOT NULL comparison operator.

solution : `select * from authors where location is not null;`

OR 
using like Operator and where :
  * The LIKE operator is used in a WHERE clause to search for a specified pattern in a column.
  * % : The percent sign represents zero, one, or multiple characters
  * _ : The underscore represents a single character

second soluiton : `select * from authors where location like '%%';`

to return any location containe seven character (not london for example) :
  using seven underscore: `select * from authors where location like '%_______%';`

#### 4. Find everyone who is not in Nazareth (including nulls)

type of comparison operators available in sql : 
  * SQL Equal (=) Operator
  * SQL Not Equal (!= or <>) Operator
  * SQL Greater Than (>) Operator
  * SQL Less Than (<) Operator
  * SQL Greater Than or Equal To (>=) Operator
  * SQL Less Than or Equal To (<=) Operator
  * SQL Not Less Than (!<) Operator
  * SQL Not Greater Than (!>) Operator

to return location is not Zararetch but without nulls values  

`select * from authors where not location = 'Nazareth';`

or 

`select * from authors where not location like 'Nazareth';`

or 

`select * from authors where not location like 'N%';`

or 

`select * from authors where location <> 'Nazareth';`

or

`select * from authors where location != 'Nazareth';`


solution : `select * from authors where location <> 'Nazareth' or location is null;`

#### 5. Find everyone with 'Wistle' in their surname (bonus points for case insensitivity)

for selecting rows with particular word : 
`select * from authors where surname like '%Wistle';`

for selecting rows with particular word with case insensitivity :
`select * from authors where Lower(surname) like lower('%Wistle%';)`

#### 6. Find the name of the publisher who released 'Python Made Easy'

join tables to get data rows from table depending on data from another teable 

`select * from publishers inner join books on books.publisher_id = publishers.id;`

this will return table cntain the publishers and books data conecting on the publisher id's

solution : `select publishers.name from publishers inner join books on books.publisher_id = publishers.id where books.name = 'Python Made Easy';`

#### 7. Find all the books published by 'No Starch Press'
solution : 
`select books.name as book_Name , publishers.name as author from publishers inner join books on books.publisher_id = publishers.id where publishers.name like 'No Starch Press';`


#### 8. Show a list of every book and their authors
 the relation between books and authors is many to many so for that the book_authors table used to break down the relation , each row in this table contain the author id and the book id , so if you look for all books by writen by a particular author we can join this table to know all the books id depending on the author id .
 
 -  first make a table from books and books
`
select * from books 
inner join book_authors 
ON 
book_authors.book_id = books.id;
`
 - second add the authors table to the result table : 
`
select * from books 
inner join book_authors 
ON 
book_authors.book_id = books.id
INNER JOIN authors
ON 
book_authors.author_id = authors.id
;`

now we get a big table contain data from three tables and we can detrmin the columns to retrun and the condtions 

solution : 
`select books.name as book_name, authors.first_name, authors.surname from books inner join book_authors ON book_authors.book_id = books.id INNER JOIN authors ON book_authors.author_id = authors.id;`

#### 9. Find all the books that Ted Burns authored

solution : `select books.name as book_name_BY_Ted from books inner join book_authors on book_authors.book_id = books.id inner join authors on book_authors.author_id = authors.id where authors.first_name like 'Ted';`

#### 10. Find everyone who wrote at least three books

first : join the authors with the book_authors and return who many every authors is repeated using count operator and do that we need to use group by operator (it wont work with out it )

`select authors.first_name , count(*) from authors join book_authors on authors.id = book_authors.author_id group by authors.first_name;`

second : we need return any author has 3 or more books so we need to use subqeury and to this we need to give it a name 'alias' 

soluiton : 
`select * from (select authors.first_name , count(*) from authors join book_authors on authors.id = book_authors.author_id group by authors.first_name) as x where count >= 3;`

OR (using having operator) : 

sol: 
`select authors.first_name from authors inner join book_authors on book_authors.author_id = authors.id group by authors.id HAVING count(book_authors.author_id) >= 3;`


#### 11. Order the publishers by the number of books they have published.

solution: 
`select publishers.name, count(books.publisher_id) from publishers inner join books on books.publisher_id = publishers.id group by publishers.name order by count(books.publisher_id) desc;`

#### 12. Find all books released after 1st Jan 1996, ordered by the number of people who wrote them.

solution: 
`select books.name, count(book_authors.book_id) from books inner join book_authors on books.id = book_authors.book_id where books.release_date > '01-Jan-96' group by books.name order by count(book_authors.book_id) desc;`


#### 13. What's the highest number of authors per book? The lowest?

the heighest : 
`select books.name, count(book_authors.book_id) from books inner join book_authors on books.id = book_authors.book_id group by books.name order by count(book_authors.book_id) desc limit 1;`

the lowest : 
`select books.name, count(book_authors.book_id) from books inner join book_authors on books.id = book_authors.book_id group by books.name order by count(book_authors.book_id) limit 1;`


#### 14. Who wrote the most books? How many did they write?

solution: 
`select authors.first_name, authors.surname, count(book_authors.author_id) as books from authors inner join book_authors on authors.id = book_authors.author_id group by authors.first_name, authors.surname order by books desc limit 1;`

## hard part : 
-- What's the average number of authors per book?
sol : 
`select AVG(count) from (select book_authors.book_id, count(book_authors.author_id) from book_authors group by book_authors.book_id) as x;`