# Fabflix
● A mock movie buying REST web app that also displays movie ratings, genres, stars.  
● Stored movie, rating, star, genres, and customer data in a MySQL database.  
● Developed a basic Android app that supports login and movie searches.  
● Increased the scalability by using three AWS instances that acts as master, slave, and load balancer.  
● Worked in a 2-person team in UCI's CS122B project course.   
Java, HTML, CSS, JavaScript, AJAX, MySQL, Tomcat, AWS, GCP.  
*Code is available upon request  

## Demo Videos
Task 1: https://drive.google.com/file/d/1JmBZVrM6eN97TRkGPfaKt6zZfDNi3vAF/view

Task 2: https://drive.google.com/drive/folders/19BnSbXmdMBYpO3HTQIVgaCPDNRN3ZVxd?usp=sharing

Task 3: https://drive.google.com/drive/folders/1fGcDkCCjPV-ONyXzNIWu_NMy8BKTZzSu?usp=sharing  
  
Task 4: https://drive.google.com/drive/folders/1GR8lzlZ4x8qBZlIJrY_TmnJ3ciF9ijKX?usp=sharing

Task 5: https://drive.google.com/drive/folders/1FtIwDblJCTe5JgEfofrWoOY8J_yImDRd?usp=sharing

## How to Deploy  
Tomcat  
user: fabflix  
password: password  
start command: /home/ubuntu/tomcat/bin/startup.sh

If tomcat is being unresponsive, please try using incognito mode and/or restart tomcat  
./shutdown.sh, ./startup.sh  

IMPORTANT  
-------------------ENABLE COOKIES--------------------

mysql -u root -p  
password: password  


## Parser Report
- Inconsistency Report Location: `/root/parser/report.txt`

### General Notes:
---
1. Converted genre 'romantic' to 'romance'.
2. Converted genre 'cops & robbers' to 'thriller'.
3. Converted genre 'suspense' to 'thriller'.

#### Casts:

1. Used `NULL` for inconsistent stage names.
2. Used `NULL` for inconsistent date of births.
3. Actors with the same `stage name` and `date of birth` in both the original database and the XML files were not added because of duplicates.
4. The `id` for new stars added into the database start with the letters 'km' (e.g. km15178).

#### Genres:
1. Used `NULL` for inconsistent genre codes.

#### Movies:
1. Used `NULL` for inconsistent film ids. 
2. Used `NULL` for inconsistent titles.
3. used `NULL` for inconsistent years.
4. Movies with same `title` and `year` in both the database and the XML files were not added to the db.

#### Actors:
1. Did not link actor to Movie if actor was not in either casts.xml or in the original database.


### Performance:
---

#### Naive Approach:
- Ran a different insert statement for each row being added to a table in the database.

- Total Time: took slightly over 5 minutes to run entire parser.


#### Optimization:
1. In-Memory HashTable
	- Loaded the relevant tables (movies, stars, genres, stars_in_movies, genres_in_movies) into an in-memory hashmap
	- `HashMap< {movieId}, {Movie object} >` where Movie object holds all needed information.
2. Batch Insert
	- Used batch insert to minimize the amount of transactions happening between the client and the database.
- Total Time: 30 seconds to run entire parser

## LIKE/ILIKE uses
- SearchServlet
    - query: `SELECT * FROM top20movies  WHERE title LIKE ?  AND year LIKE ?  AND director LIKE ?  AND starsName LIKE ? ORDER BY rating DESC, title DESC`
    - ? = `"%" + titleId + "%"` (for example)
- MoviesResultServlet
    - ? = `id + "%";` (for first letter matching)
    - ? = `"%" + id + "%";` (for genres)

## Contributions  
Kai Benothman
- Project 1, 2:
    - Did the vast majority of managing the AWS instance
- Project 1
    - Most of the java files: MoviesServlet, SingleMovieServlet, SingleStarServlet   
    - Most of the js/html files: movie-list, single-movie, single-star 
    - Did the video demo recording 
- Project 2
    - CartServlet, LoginFilter, PayServlet, User, VerifyPay, cart.html/js, pay.html/js (all the cart/payment/checkout process, session id for user)
    - Added upon the Jerry's Login to work with session id and having approprite error messages
    - Got the Search form to functionally work (SearchServelet, search.html/js)
    - Did the vast majority of the CSS of the website
    - Did the video demo recording 
- Project 3
    - All of Task 7
- Project 4
    - All of Task 1 (full-text search & autocomplete)  
- Project 5
    - All of Task 1 and 2 (Connection Pooling and Master/Slave replication)  
    - Load Balancer  
      
Jerry Shen
- Project 1
    - Most of the MySQL query related tasks (Servlet files, creating the database)   
    - Helped out with the above java/js/html files  
- Project 2  
    - Most of the MySQL query related tasks (Servlet files, creating the tables, such as for star popularity and genres sorting)  
    - Got the Login form to functionally work (LoginServelet, login.html/js)  
    - MoviesResultServlet, GenresServlet, movie-list.html/js (browsing via title/genres with sorting and pagination)  
    - Added upon Kai's browsing to have sorting support and buttons on front-end  
- Project 3  
    - Added the reCaptcha  
    - All of Task 6  
    - Did the video demo recording   
- Project 4  
    - All of Task 2 (Android App)    
    - Did the video demo recording    
- Project 5   
    - Set up the GCP instance  
    - Did Task 4.1 (TS/TJ measuring and log_processing.py script)  

## Connection Pooling
#### Include the filename/path of all code/configuration files in GitHub of using JDBC Connection Pooling.
- /fabflix/WebContent/META-INF/context.xml
- /fabflix/src/BrowseServlet.java
- /fabflix/src/CartServlet.java
- /fabflix/src/DashBoardMovieServlet.java
- /fabflix/src/DashBoardStarServlet.java
- /fabflix/src/DashBoardTableServlet.java
- /fabflix/src/GenresServlet.java
- /fabflix/src/LoginServlet.java
- /fabflix/src/MoviesResultsServlet.java
- /fabflix/src/MoviesServlet.java
- /fabflix/src/PayServlet.java
- /fabflix/src/SearchServlet.java
- /fabflix/src/SearchSuggestion.java
- /fabflix/src/SingleMovieServlet.java
- /fabflix/src/SingleStarServlet.java
- /fabflix/src/StarsServlet.java
- /fabflix/src/UpdateSecurePassword.java
- /fabflix/src/VerifyPay.java	
 
 #### Explain how Connection Pooling is utilized in the Fabflix code.
 - By providing the username and password of a MySQL user once in a resource tag in a `context.xml` file, JDBC can use those credentials to utilize connection pooling. Whenever one of our servlets sends a query to the database, using the credentials found in the context file, it simply reuses one of the connections from the "pool" and simply disconnects, or closes the connection when it is done (instead of physically opening a connection and then physically closing it every-time).
	
#### Explain how Connection Pooling works with two backend SQL.
- The "connection pooling" module is a layer that sits on top of the standard JDBC driver. Each backend has a set of allocated connections that can be reused and the JDBC driver maintains and facilitates which connections should be opened or closed.
    

## Master/Slave
#### Include the filename/path of all code/configuration files in GitHub of routing queries to Master/Slave SQL.
- /fabflix/WebContent/META-INF/context.xml
- /fabflix/src/BrowseServlet.java
- /fabflix/src/CartServlet.java
- /fabflix/src/DashBoardMovieServlet.java
- /fabflix/src/DashBoardStarServlet.java
- /fabflix/src/DashBoardTableServlet.java
- /fabflix/src/GenresServlet.java
- /fabflix/src/LoginServlet.java
- /fabflix/src/MoviesResultsServlet.java
- /fabflix/src/MoviesServlet.java
- /fabflix/src/PayServlet.java
- /fabflix/src/SearchServlet.java
- /fabflix/src/SearchSuggestion.java
- /fabflix/src/SingleMovieServlet.java
- /fabflix/src/SingleStarServlet.java
- /fabflix/src/StarsServlet.java
- /fabflix/src/UpdateSecurePassword.java
- /fabflix/src/VerifyPay.java

#### How read/write requests were routed to Master/Slave SQL?
- Read requests can be routed to both the master and the slave instances because it does not affect the database content at all. On the other hand, write requests must always be routed to the master in order for the data to show up in all other slave instances. In all of our servlets that write to the database, another Resource that only connects to the master instance (using the master instance private IP) is used instead of the default resource which connects to any instance.

## JMeter TS/TJ Time Logs
#### Instructions of how to use the `log_processing.*` script to process the JMeter logs.
- [log_processing.py](https://github.com/UCI-Chenli-teaching/cs122b-spring20-team-76/blob/master/log_processing.py)  
The log_output.txt file should be in /home/ubuntu/tomcat/webapps/cs122b-spring20-team-76  
Move/make a copy of the log_output.txt to where the file path of where the log_output.txt is in the Python script is correct:  
Make sure the file path code in the Python script matches to your system.  

```
file = open("c:/Users/Admin/Desktop/cs122b/cs122b-spring20-team-76/log_output.txt", "r")
```
Please use Python 3+ (We used Python 3.7)  
The Python script will print out the average TS & TJ times.
