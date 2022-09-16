# Semantic Search 

Traditional database tables only allow users to search text and numbers using keyword matching or full text search. By default, machine learning database tables store text and image as embeddings, making it easy for users to perform semantic search with simple SQL statement.

!!! warning "Prerequisite"

		If you like to enable semantic search for your columns, make sure the column is created using data type `TEXT`

Semantic Search can be find information using the meaning of the words. For example, let's say you want to search for `ganster` movies, you can run the following query

```
SELECT mldb.movie._id, mldb.movie.title, mldb.movie.overview, predictions.score
FROM mldb.movie
JOIN model.semantic_search
ON model.semantic_search.inputs = mldb.movie.overview
WHERE mldb.movie._id < 20
AND model.semantic_search.similar = 'gangster'
ORDER BY predictions.score DESC
```


| _id    | title         | overview                                                                 | score    |
| -----  | -----------   | -----------------------------------------------                          | --------- |
| 7      | Pulp Fiction  | The lives of two mob hitmen, a boxer, a gangster and his wife, and a pair of diner bandits intertwine in four tales of violence and redemption. | 0.61      | 
| 16     | Goodfellas    | The story of Henry Hill and his life in the mob, covering his relationship with his wife Karen Hill and his mob partners Jimmy Conway and Tommy DeVito in the Italian-American crime syndicate. | 0.39      |
| 10     | Fight Club    | An insomniac office worker and a devil-may-care soapmaker form an underground fight club that evolves into something much, much more. | 0.39      |
