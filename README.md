practiceSG1
===========

-- last time i practiced sql was a few days ago. 
this is how far i am. let me know if there is anything 
i could improve please! thank you, jesse! 12.01.14



-- A list of the top 10 US cities by average invoice size
SELECT billing_city AS city, billing_state AS state,
  ROUND (AVG(total),0) AS average_invoice_size
FROM invoices
WHERE billing_country = 'USA'
GROUP BY city
ORDER BY average_invoice_size
DESC LIMIT 10;

-- A list of the top 3 cities in California by number of invoices
SELECT billing_city, billing_state,
  COUNT(*) AS most_invoices
FROM invoices
WHERE billing_state = 'CA'
GROUP BY billing_state
ORDER BY most_invoices
DESC LIMIT 3;

-- A list of the top 3 cities in California by gross sales
SELECT billing_city, billing_state,
  SUM(total) AS grossest_sales
FROM invoices
WHERE billing_state = 'CA'
GROUP BY billing_state
ORDER BY grossest_sales
DESC LIMIT 3;

-- A list of the top 3 cities in California by average invoice size
SELECT billing_state, billing_city,
  AVG(total) AS average_total_of_the_invoice
FROM invoices
WHERE billing_state = 'CA'
GROUP BY billing_state
ORDER BY average_total_of_the_invoice
DESC LIMIT 3;

-- A list of the top 3 countries by total number of customers
SELECT country,
  COUNT(*) AS total_customers
FROM customers
GROUP BY country
ORDER BY total_customers
DESC LIMIT 3;

-- A list of the top 7 cities (anywhere) by total number of customers
SELECT city, country,
  COUNT(*) AS customers
FROM customers
GROUP BY id
ORDER BY customers
DESC LIMIT 7;



-- The 5 customers who most recently purchased something
SELECT customers.id, customers.first_name,
customers.last_name, customers.email,
invoices.invoice_date, invoices.total
FROM customers
JOIN invoices
ON (invoices.customer_id = customers.id)
ORDER BY invoices.invoice_date 
DESC LIMIT 5;


-- The top 10 customers by total number of orders
SELECT customers.id, customers.first_name,
customers.last_name, customers.email,
COUNT(*) AS order_count
FROM customers
JOIN invoices
ON (invoices.customer_id = customers.id)
GROUP BY customers.id
ORDER BY order_count
DESC LIMIT 10;



-- The top 10 customers by gross sales
SELECT customers.id, customers.first_name,
customers.last_name, customers.email,
SUM(total) AS gross_sales
FROM customers
JOIN invoices
ON (invoices.customer_id = customers.id)
GROUP BY customers.id
ORDER BY gross_sales
DESC LIMIT 10;


-- The top 5 genres by number of tracks (fill in the blanks)
SELECT genres.name, genres.id, 
COUNT(*) AS track_count
FROM genres
JOIN tracks
ON (tracks.genre_id = genres.id)
GROUP BY tracks.genre_id
ORDER BY track_count
DESC LIMIT 5;



-- The top 5 genres by total track length (in milliseconds)
SELECT genres.id, genres.name,
SUM(milliseconds) AS total_time
FROM genres
JOIN tracks
ON (genres.id = tracks.genre_id)
GROUP BY genres.id
ORDER BY total_time
DESC LIMIT 5;


-- The top 5 genres by average track length (in milliseconds)
SELECT genres.id, genres.name,
ROUND(AVG(milliseconds),0) AS average_track_time
FROM genres
JOIN tracks
ON (genres.id = tracks.genre_id)
GROUP BY tracks.milliseconds
ORDER BY average_track_time
DESC LIMIT 5;


-- The top 5 albums by total track length
-- Hint: you'll need to JOIN the albums table and the tracks table
-- Hint: the tracks table has an album_id field
SELECT albums.id, albums.title,
tracks.id, tracks.milliseconds, 
artists.id, artists.name,
SUM(tracks.album_id) AS album_duration
FROM tracks
JOIN albums
ON (tracks.album_id = albums.id)
JOIN artists
ON (albums.artist_id = artists.id)
GROUP BY albums.id
ORDER BY album_duration
DESC LIMIT 5;


-- The top 5 albums by average track length
SELECT albums.id, albums.title, 
tracks.id,
ROUND(AVG(milliseconds),0) AS albums_with_the_highest_average_track_lengths
FROM albums
JOIN tracks
ON (albums.id = tracks.id)
GROUP BY tracks.milliseconds
ORDER BY albums_with_the_highest_average_track_lengths
DESC LIMIT 5;

-- The top 5 albums by total album price
-- Hint: the "tracks" table has a unit_price field, so the "price" of an album
--       is the sum of its tracks' unit_price fields.
SELECT albums.id, albums.title,
tracks.album_id, 
SUM(unit_price) AS album_price
FROM tracks
JOIN albums
ON (tracks.album_id = albums.id)
GROUP BY unit_price
ORDER BY album_price
DESC;



-- The 10 albums with the longest play-time
SELECT albums.id, albums.title,
SUM(milliseconds) AS longest_album
FROM tracks
JOIN albums
ON (tracks.id = albums.id)
GROUP BY milliseconds
ORDER BY longest_album
DESC LIMIT 10;



