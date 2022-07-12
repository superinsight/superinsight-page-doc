
## What is ML table
ML table are database tables that are optimize for ML predictions. Traditional SQL tables store datatypes such as `STRING`, `NUMBER` and `DATETIME`, which works fine for simple data retrieval.
However, for ML operations are done using more complex datatype such as `EMBEDDINGS`. ML table not only store traditional datatypes, it also convert and index them as `EMBEDDINGS` in so the data are ready for ML operations.

## Working a ML table
Working with ML table is easy in Superinsight, just use the same SQL sytnax you would use for other SQL databases

#### Creating a table with data from another database table
```
CREATE TABLE superinsight.my_mldb.movie_reviews
  AS (SELECT id, review
      FROM demo_db.my_database.movie_reviews
      WHERE movie_id < 3);
```

#### Dropping a table
```
DROP TABLE superinsight.my_mldb.movie_reviews
```