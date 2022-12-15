#### Nba Player Stats Web Scraping ####

My project is to mainly fetch specific players and recieve an output 
stating the average points for game from the amounts of games played. 

This project imports several libraries, including pandas, requests, datetime, numpy, json, pymysql, and secrets.

This projecte stablishes a connection to a MySQL database on mysql.clarksonmsda.org using the pymysql library, and creates a cursor to interact with the database.

This project sets an option in pandas to display all columns in a dataframe.

This project defines a URL to an NBA stats API and uses the requests library to make a GET request to that API.

This project processes the API response and extracts certain data from it.

This project creates an empty dictionary called result and adds data from the API response to it.

Made the HTTP request to the NBA stats API and parse the JSON response

I use a dictionary comprehension to extract the data from the response and add it to a dictionary

I've created the table in sql 

Queried the API for player stats data

Created empty list to hold player names

Created an empty list to hold the values for each row of data

Iterated over the rows of data in the API response

Gotten the player name and games played from the API response

Added the player name to the list

Added the values for this row of data to the list of values

Use placeholder in INSERT statement

Executed the INSERT statement multiple times with different parameter values