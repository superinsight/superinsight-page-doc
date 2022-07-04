
## SQL Query with ML predictions
ML tables are setup in a way that it allows you to perform semantic search using traditional SQL statement

## Traditional Query

Let's say you want to find positive movie reviews, and you run the following query 
```
SELECT * 
FROM demo_db.my_database.movie_reviews
WHERE review like '%good%'
```

!!! fail "Failed"
    The result came back with the review `This is the worst movie I ever saw, it's not good`. This attempt failed because traditional SQL query only uses keyword mathcing and it sees the word `good` in there

    | id            | movie_id    | review                                                           |
    | -----------   | ----------- | ------------------------------------                             |
    | 4             | 3           | This is the worst movie I ever saw, it's not good                |



## Query with Semantic Search

Using ML datbase to perform semantic search is easy, just use SQL query to search your ML table and you will you get a much more accurate results with relevance score as part of the result set
```
SELECT * 
FROM superinsight.my_mldb.movie_reviews
WHERE review = '%rating for the movie was great%'
AND _score > 0.5
```

!!! success "Success"
    ML query is able to give you a much more accurate results because it does not rely on keyword matching, instead it uses our build in ML models for inference.

    | id            | review                                                                | _score    |
    | -----------   | -----------------------------------------------                       | --------- |
    | 3             | This movie is awesome, it is probably the best movie of the year      | 0.728     | 
    | 5             | The actor did a great job with his part and I give it thumbs up       | 0.603     |
