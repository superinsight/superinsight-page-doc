# Quickstart
This is a quickstart guide on how to use Superinsight ML database to perform ML operations using SQL query.
You will learn how to create a database table and insert data into your table.
Finally you will be able to run a semantic search using SQL query. It's that simple.
If you don't have an account yet, sign up to [get early access](https://www.superinsight.ai/request-a-demo) here.


## 1. Connect to your host
After your account has been created, you will find a dedicated host on your Superinsight Cloud Studio.
Using this host, you will be able to connect to your database

=== "Superinsight Cloud Studio"

    If you are using Superinsight Cloud Studio, your host should be already connected to your account. Simply login and click on SQL Editor to begin.

=== "Third Party Tools"

    If you wish to connect to using third party tools, we recommend using DBeaver as your editor. You can use the postgresql connection.
    ```
    postgresql://{username}:{password}@{host}:{port}/superinsight
    ```

## 2. Create a database table

Database table can be created using the `CREATE TABLE` statement in superinsight. For the columns that you will like to enable semantic search, assign them with the data type `TEXT`.
```
CREATE TABLE mldb.movie (
	_id serial PRIMARY KEY,
	title TEXT,
    genre TEXT,
	overview TEXT,
	released_year varchar(100),
	runtime varchar(100),
	imdb_rating float8,
	director varchar(100)
	no_of_votes int4
);
```

## 3. Insert data to database table 

Rows can be inserted to the database using the `INSERT` statement in superinsight.
```
INSERT INTO mldb.movie(title, genre, overview, released_year, runtime, imdb_rating, director, no_of_votes)
VALUES ('The Shawshank Redemption', 'Drama', 'Two imprisoned men bond over a number of years, finding solace and eventual redemption through acts of common decency.', '1994', '142 min', 9.3, 'Frank Darabont', 2343110);

INSERT INTO mldb.movie(title, genre, overview, released_year, runtime, imdb_rating, director, no_of_votes)
VALUES ('The Godfather', 'Crime, Drama', 'An organized crime dynasty's aging patriarch transfers control of his clandestine empire to his reluctant son.', '1972', '175 min', 9.2, 'Francis Ford Coppola', 1620367);

INSERT INTO mldb.movie(title, genre, overview, released_year, runtime, imdb_rating, director, no_of_votes)
VALUES ('The Dark Knight', 'Action, Crime, Drama', 'When the menace known as the Joker wreaks havoc and chaos on the people of Gotham, Batman must accept one of the greatest psychological and physical tests of his ability to fight injustice.', '2008', '152 min', 9, 'Christopher Nolan', 2303232);

```

## 4. Performing semantic search on ML table

Semantic search can be executed using the keyword `SIMILAR` in a standard query
```
SELECT * FROM mldb.movie WHERE overview SIMILAR 'gangster'
```

You should see the following results

| _id           | title                     | overview                                                                 | _score    |
| -----------   | -----------               | -----------------------------------------------                          | --------- |
| 3             | The Godfather             | An organized crime dynasty's aging patriarch transfers control of his clandestine empire to his reluctant son. | 0.72      | 
| 2             | The Shawshank Redemption  | Two imprisoned men bond over a number of years, finding solace and eventual redemption through acts of common decency. | 0.28      |
| 1             | The Dark Knight           | When the menace known as the Joker wreaks havoc and chaos on the people of Gotham, Batman must accept one of the greatest psychological and physical tests of his ability to fight injustice. | 0.19      |


## 5. Review and learn more

As you can tell, semantic search allows us to search content in our data using the meaning of the word. Standard SQL queries are also supported so you can perform additional filtering using operators such as `=` and `LIKE`.

If you like to learn more by examples, please continue with our [Operation Guide](/operations) 