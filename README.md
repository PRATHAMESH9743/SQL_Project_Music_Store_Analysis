# SQL_Project_Music_Store_Analysis

SQL project to analyze online music store data

The Goal of this project is to analyze the music playlist database. and to examine the dataset with SQL and help the store understand its business growth by answering simple questions.

## Database and Tools
MySQL

## Problem Statement for SQL Project: Music Store Analysis
The objective of this SQL project is to analyze a music store database and extract valuable insights to inform business decisions. 
The database contains information about employees, country, Genre, Artist, Track, Playlist, Playlist track, media type, invoices, invoices line, cities, and customers.

The project aims to address the following key questions:
1.Identifying Senior Employees:

Determine the senior-most employee based on their job title within the organization.

2.Analyzing Invoice Distribution:

Identify countries with the highest number of invoices, providing insights into market distribution and sales trends.

3.Top 3 Invoice Values:

Retrieve the top three values of total invoices, allowing for an understanding of significant revenue-generating transactions.

4.Optimal City for Music Festival Promotion:

Determine the city with the highest sum of invoice totals, as this city will be selected for a promotional Music Festival. Return both the city name and the sum of all invoice totals.

5.Recognizing the Best Customer:

Identify the customer who has spent the most money, declaring them the best customer. Generate a query that returns the customer with the highest total spending.

6.Rock Music Listeners:

Create a query to retrieve the email, first name, last name, and genre of all listeners who prefer Rock music. The list should be ordered alphabetically by email, starting with 'A'.

7.Inviting Top Rock Bands:

Identify and invite artists who have written the most rock music in the dataset. Develop a query that returns the artist name and the total track count for the top 10 rock bands.

8.Longer-than-Average Tracks:

Generate a query to retrieve all track names with a song length longer than the average song length. Include the name and duration in milliseconds for each track, ordering them by the song length with the longest songs listed first.


##SQL Query
/* Q1: Who is the senior most employee based on job title? */

SELECT title, last_name, first_name 
FROM employee
ORDER BY levels DESC
LIMIT 1


/* Q2: Which countries have the most Invoices? */

SELECT COUNT(*) AS c, billing_country 
FROM invoice
GROUP BY billing_country
ORDER BY c DESC


/* Q3: What are top 3 values of total invoice? */

SELECT total 
FROM invoice
ORDER BY total DESC


/* Q4: Which city has the best customers? We would like to throw a promotional Music Festival in the city we made the most money. 
Write a query that returns one city that has the highest sum of invoice totals. 
Return both the city name & sum of all invoice totals */

SELECT billing_city,SUM(total) AS InvoiceTotal
FROM invoice
GROUP BY billing_city
ORDER BY InvoiceTotal DESC
LIMIT 1;


/* Q5: Who is the best customer? The customer who has spent the most money will be declared the best customer. 
Write a query that returns the person who has spent the most money.*/

SELECT customer.customer_id, first_name, last_name, SUM(total) AS total_spending
FROM customer
JOIN invoice ON customer.customer_id = invoice.customer_id
GROUP BY customer.customer_id
ORDER BY total_spending DESC
LIMIT 1;


/* Q6: Write query to return the email, first name, last name, & Genre of all Rock Music listeners. 
Return your list ordered alphabetically by email starting with A. */

/*Method 1 */

SELECT DISTINCT email,first_name, last_name
FROM customer
JOIN invoice ON customer.customer_id = invoice.customer_id
JOIN invoiceline ON invoice.invoice_id = invoiceline.invoice_id
WHERE track_id IN(
	SELECT track_id FROM track
	JOIN genre ON track.genre_id = genre.genre_id
	WHERE genre.name LIKE 'Rock'
)
ORDER BY email;


/* Method 2 */

SELECT DISTINCT email AS Email,first_name AS FirstName, last_name AS LastName, genre.name AS Name
FROM customer
JOIN invoice ON invoice.customer_id = customer.customer_id
JOIN invoiceline ON invoiceline.invoice_id = invoice.invoice_id
JOIN track ON track.track_id = invoiceline.track_id
JOIN genre ON genre.genre_id = track.genre_id
WHERE genre.name LIKE 'Rock'
ORDER BY email;


/* Q7: Let's invite the artists who have written the most rock music in our dataset. 
Write a query that returns the Artist name and total track count of the top 10 rock bands. */

SELECT artist.artist_id, artist.name,COUNT(artist.artist_id) AS number_of_songs
FROM track
JOIN album ON album.album_id = track.album_id
JOIN artist ON artist.artist_id = album.artist_id
JOIN genre ON genre.genre_id = track.genre_id
WHERE genre.name LIKE 'Rock'
GROUP BY artist.artist_id
ORDER BY number_of_songs DESC
LIMIT 10;


/* Q8: Return all the track names that have a song length longer than the average song length. 
Return the Name and Milliseconds for each track. Order by the song length with the longest songs listed first. */

SELECT name,miliseconds
FROM track
WHERE miliseconds > (
	SELECT AVG(miliseconds) AS avg_track_length
	FROM track )
ORDER BY miliseconds DESC;

SELECT title, last_name, first_name 
FROM employee
ORDER BY levels DESC
LIMIT 1
