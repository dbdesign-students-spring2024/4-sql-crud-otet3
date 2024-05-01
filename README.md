# SQL CRUD

# RESTAURANTS TABLE
CREATE TABLE restaurants (
  id INTEGER PRIMARY KEY,
  name TEXT,
  neighborhood TEXT,
  cuisine TEXT,
  price_range TEXT,
  good_for_kids BOOLEAN,
  opening_time TEXT,
  closing_time TEXT
);

# REVIEWS TABLE
CREATE TABLE reviews (
  id INTEGER PRIMARY KEY,
  restaurant_id INTEGER,
  reviewer_name TEXT,
  rating INTEGER,
  content TEXT,
  FOREIGN KEY (restaurant_id) REFERENCES restaurants(id)
);

.mode csv
.import restaurants.csv restaurants

## 1
SELECT * 
FROM restaurants
WHERE neighborhood = 'Brooklyn' AND price_range = 'Cheap';
 
## 2
SELECT r.name, AVG(v.rating) AS avg_rating
FROM restaurants r
JOIN reviews v ON r.id = v.restaurant_id  
WHERE r.cuisine = 'China'
GROUP BY r.id
HAVING AVG(v.rating) >= 3
ORDER BY avg_rating DESC;
 
## 3
SELECT *
FROM restaurants
WHERE strftime('%H:%M', 'now', 'localtime') BETWEEN opening_time AND closing_time;

## 4
INSERT INTO reviews (restaurant_id, reviewer_name, rating, content)
VALUES (1, 'John Smith', 4, 'Great food and excellent service!');

## 5
DELETE FROM restaurants 
WHERE good_for_kids = 0;

## 6
SELECT neighborhood, COUNT(*) AS num_restaurants
FROM restaurants
GROUP BY neighborhood;

# PART 2
CREATE TABLE users (
  id INTEGER PRIMARY KEY,
  email TEXT UNIQUE,
  password TEXT, 
  username TEXT
);

CREATE TABLE posts (
  id INTEGER PRIMARY KEY,
  user_id INTEGER,
  content TEXT,
  type TEXT,
  recipient_id INTEGER,
  created_at DATETIME DEFAULT CURRENT_TIMESTAMP,
  deleted_at DATETIME,
  FOREIGN KEY (user_id) REFERENCES users(id),
  FOREIGN KEY (recipient_id) REFERENCES users(id)
);


