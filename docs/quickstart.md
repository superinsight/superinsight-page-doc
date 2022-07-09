# Quickstart
This is a quickstart guide on how to use Superinsight ML database to perform ML operations using SQL query.
You will learn how to connect to your existing database table and populate data into your new ML table.
Finally you will be able to run a semantic search using SQL query. It's that simple.
If you don't have an account yet, please [sign up for a free trial](signup) so we can get you started.


## 1. Connect to your host
After your account has been created, you will find a dedicated host on your Superinsight Cloud Studio.
Using this host, you will be able to connect to existing databases and create ML databases.

=== "Superinsight Cloud Studio"

    If you are using Superinsight Cloud Studio, your host should be already connected to your account. Simply login and click on SQL Editor to begin.

=== "Third Party Tools"

    If you wish to connect to your host using other tools such DBeaver or Superset, you can use the trino jdbc protocols.
    Note that most third party tools only support `SELECT` commands. For complete full features, we recommend using Superinsight Cloud Studio.
    ```
    jdbc:trino://username:password@yourhost.superinsight.dev:8080
    ```

## 2. Attach an existing databases

Click on the `[Attach Database]` button and connect using your database connection. You can also use our demo database connection string below and use the name `demo_db`.
```
postgresql://demo_user:demo_password@demo_db.superinsight.dev/demo
```
If you like to attach your existing database, see the list of our [Supported Databases](/setup/database) and their connection strings.



## 3. Querying data from existing database

After connecting to a database you execute a standard `SELECT` statement to retrieve and should see the following results

```
SELECT * 
FROM demo_db.my_database.movie_reviews
LIMIT 3;
```


| id            | movie_id    | review                                                           |
| -----------   | ----------- | ------------------------------------                             |
| 1             | 1           | This is the worst movie I ever saw because the plot is horrible  |
| 2             | 1           | The story has no point, the main character was too weak          |
| 3             | 2           | This movie is awesome, it is probably the best movie of the year |


## 4. Create a ML database in Superinsight

Click on the `[Create Database]` button to create a new ML database and name it `my_mldb`.
The biggest difference between a traditional database table and a Superinsight ML database table is that in ML tables are indexed in a way that allows you to perform semantic search using SQL query.

## 5. Create a ML table from existing data

```
CREATE TABLE superinsight.my_mldb.movie_reviews
  AS (SELECT id, review
      FROM demo_db.my_database.movie_reviews
      WHERE movie_id < 3);
```


## 6. Performing semantic search on ML table

After moving data to your ML table, you execute a standard `SELECT` statement to perform semantic search
```
SELECT * 
FROM superinsight.my_mldb.movie_reviews
WHERE review = '%rating for the movie was great%'
LIMIT 3;
```

You should see the following results

| id            | review                                                                | _score    |
| -----------   | -----------------------------------------------                       | --------- |
| 3             | This movie is awesome, it is probably the best movie of the year      | 0.72      | 
| 2             | The story has no point, the main character was too weak               | 0.28      |
| 1             | This is the worst movie I ever saw because the plot is horrible       | 0.19      |
