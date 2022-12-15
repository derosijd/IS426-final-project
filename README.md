# IS426-final-project
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



import pandas as pd 
import requests 
import datetime 
import numpy as np
import json 
import pymysql
import secrets 

host='mysql.clarksonmsda.org'
user='is426'
passwd='is426clarkson'

conn = pymysql.connect(host='mysql.clarksonmsda.org', port=3306, user='is426',
                       passwd='is426clarkson', db='is426', autocommit=True)

cur = conn.cursor(pymysql.cursors.DictCursor)


pd.set_option('display.max_columns', None)

test_url = 'https://stats.nba.com/stats/leagueLeaders?LeagueID=00&PerMode=PerGame&Scope=S&Season=2022-23&SeasonType=Regular%20Season&StatCategory=PTS'


r = requests.get('https://stats.nba.com/stats/leagueLeaders?LeagueID=00&PerMode=PerGame&Scope=S&Season=2022-23&SeasonType=Regular%20Season&StatCategory=PTS')
#print(r.text)

result={}
data = json.loads(r.text)
for row in data['resultSet']['rowSet']:
    result[row[2]]= [row[5],row[-2],row[10],row[18],row[19]]

response = requests.get(url=test_url).json()

#Create the table
create_table_query = """
CREATE TABLE NBA_Player_Stats (
    player_name VARCHAR(255),
    games_played INT,
    points INT,
    rebounds INT,
    assists INT,
    three_points_Made INT
)
"""

#Execute the query
cur.execute(create_table_query)

# Query the API for player stats data
response = requests.get(url=test_url).json()

# Create empty list to hold player names
player_names = []

# Create an empty list to hold the values for each row of data
values = []

# Iterate over the rows of data in the API response
for row in response['resultSet']['rowSet']:
    # Get the player name and games played from the API response
    player_name = row[2]
    games_played = row[5]
    points = row[-2]
    rebounds = row[18]
    assists = row[19]
    three_points_Made = row[10]
    
    # Add the player name to the list
    player_names.append(player_name)
    
    # Add the values for this row of data to the list of values
    values.append((games_played, player_name, points, rebounds, assists, three_points_Made))

# Use placeholder in INSERT statement
insert_query = "INSERT INTO NBA_Player_Stats (games_played, player_name, points, rebounds, assists, three_points_Made, Date_pulled) VALUES (%s, %s, %s, %s, %s, %s, NOW())"


# Execute the INSERT statement multiple times with different parameter values
cur.executemany(insert_query, values)
