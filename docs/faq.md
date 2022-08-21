# FAQ
Here are a list of frequently asked questions that are commonly asked

??? quote "What a machine learning database?"

    Machine learning is a database that has machine learning features build-in to the database.
		Common ML operations such as model training, predictions and semantic search are available inside the database.

??? quote "What is semantic search?"

    Semantic search matches a user query to the related material, semantic search uses user intent, context, and conceptual meanings. It use vector search and machine learning to offer results that attempt to match a user's query, even when no word matches exist.


??? quote "How is semantic search different than select query?"

    Let's say you want to find positive movie reviews, and you run the following query
			```
			SELECT * 
			FROM mldb.movie_reviews
			WHERE review like '%good%'
			```

    The result came back with the review `This is the worst movie I ever saw, it's not good`. This attempt failed because traditional SQL query only uses keyword mathcing and it sees the word `good` in there

    | id            | movie_id    | review                                                           |
    | -----------   | ----------- | ------------------------------------                             |
    | 4             | 3           | This is the worst movie I ever saw, it's not good                |


    Columns with data type `TEXT` are automatically indexed for semantic search purposes.
		To perform semantic search, use the query to find results base on the meaning of the sentence. 
			```
            SELECT mldb.movie_reviews._id, mldb.movie_reviews.review, predictions.score
            FROM mldb.movie_reviews
            JOIN model.semantic_search on model.semantic_search.inputs = mldb.movie_reviews.review
            JOIN model.semantic_search on model.semantic_search.similar = ['rating for the movie was great']
            AND predictions.score > 0.5
            WHERE mldb.movie._id < 3


			```

    | _id           | review                                                                | score    |
    | -----------   | -----------------------------------------------                       | --------- |
    | 3             | This movie is awesome, it is probably the best movie of the year      | 0.728     | 
    | 5             | The actor did a great job with his part and I give it thumbs up       | 0.603     |

??? quote "What is a machine learning prediction?"

    In machine learning, prediction refers to the output of an algorithm after it has been trained on previous data and applied to fresh data to estimate the likelihood of a specific result.


??? quote "What is a machine learning model training?"

    Training a machine learning (ML) model is the process of providing training data to a machine learning algorithm so that it can learn. Following the creation of the model, it may be used to generate predictions for comparable circumstances.
		
		