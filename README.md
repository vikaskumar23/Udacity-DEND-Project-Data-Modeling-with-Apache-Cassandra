## Udacity Data Engineering Project : Data Modeling with Cassandra
The objective of this project is to create a Apache Cassandra NoSql analytical database for a music startup sparkify. The database is to identify which songs users are listening to and how people are interacting with the sparkify music listening app.

## Introduction
In this project a analytical database is created on Apache Cassandra. Currently the data is present in csv files. So a ETL job is developed that takes data from files, transform it and push the data into cassandra keyspace, so that data can be easily queried.

## Project Scope
The analytics Team wants to execute below three queries on database.

1. Give me the artist, song title and song's length in the music app history that was heard during sessionId = 338, and itemInSession = 4.
2. Give me only the following: name of artist, song (sorted by itemInSession) and user (first and last name) for userid = 10, sessionid = 182
3. Give me every user name (first and last) in my music app history who listened to the song 'All Hands Against His Own'

## Create table query and Description of Partition key and Clustering Key for each query
**Query 1**:
	```
	query = "CREATE TABLE IF NOT EXISTS songs "
	query = query + "(sessionId int, itemInSession int, artist text, song text, length decimal, PRIMARY KEY (sessionId, itemInSession))"
	session.execute(query)
	```
	**Query Description**
>	For this query Primary Key, sessionId is used as partion key and itemInSession is used as clustering key. This allows Data partitioning by sessionId and clustering by itemInSession which helps in fast retrieval of data for a specific sessionId.

**Query 2**:
	```
	query = "CREATE TABLE IF NOT EXISTS songplay_songdata "
	query = query + "(userId int, sessionId int, artist text, song text, first_name text, last_name text, itemInSession int, PRIMARY KEY ((userId, sessionId), itemInSession))"
	session.execute(query)
	```
	**Query Description**
> For this query Primary Key, userid is used as partition key and sessionId is used as clustering key. Due to this data is partitioned by userId and clustered by sessionId. This helps in fast retrieval of data for specific userId. And data is clustered by sessionId so it returns data sorted by sessionId.
	
**Query 3**:
	```
	query = "CREATE TABLE IF NOT EXISTS songplay_songdata "
	query = query + "(userId int, sessionId int, artist text, song text, first_name text, last_name text, itemInSession int, PRIMARY KEY ((userId, sessionId), itemInSession))"
	session.execute(query)
	```
	**Query Description**
> For this query query Primary Key, song name is selected as partion key which allows easy retrieval of data for specific song name, but this only can't identify data uniquely in table. So user_id is used as clustering key, as user id is unique for every user. This makes each row unique and all data for specific song name can easily fetched from database.

## Dataset Required for Project
For this project, we'll be working with one dataset: ```event_data```. The directory of CSV files partitioned by date. Here are examples of filepaths to two files in the dataset:

    ```
	event_data/2018-11-08-events.csv
	event_data/2018-11-09-events.csv
    ```

## Project Execution
Just run the queries in notebook one by one. Make sure the keyspace is created and set before executing Create table queries.