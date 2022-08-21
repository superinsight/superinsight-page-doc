# Semantic Search 

You can perform semantic search for any columns with data type `TEXT` using the operator `SIMILAR`.

```
SELECT mldb.movie._id, mldb.movie.title, mldb.movie.overview, predictions.score
FROM mldb.movie
JOIN model.semantic_search on model.semantic_search.inputs = mldb.movie.overview
JOIN model.semantic_search on model.semantic_search.similar = ['gangster']
WHERE mldb.movie._id < 3
```


| _id           | title                     | overview                                                                 | _score    |
| -----------   | -----------               | -----------------------------------------------                          | --------- |
| 3             | The Godfather             | An organized crime dynasty's aging patriarch transfers control of his clandestine empire to his reluctant son. | 0.72      | 
| 2             | The Shawshank Redemption  | Two imprisoned men bond over a number of years, finding solace and eventual redemption through acts of common decency. | 0.28      |
| 1             | The Dark Knight           | When the menace known as the Joker wreaks havoc and chaos on the people of Gotham, Batman must accept one of the greatest psychological and physical tests of his ability to fight injustice. | 0.19      |
