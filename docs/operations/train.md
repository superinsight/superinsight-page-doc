
## Train custom models
To train your own custom model, you can choose a pretrained ML model as a based model, and finetuned it with your own data

| Task            				| Description                                                         										| Model Name 										|
| ----------------------- | -----------------------------------------------                       									| ----------------------------- |
| Text Classification     | Use to assign label or class to text, labels such as sentiment or product category			| model.text_classification     | 
| Question Answering      | Use to find answers in a large amount of text    																			  | model.question_answering      | 
| Recommender             | Use to recommend items to user base on collaborative filtering								          | model.recommender             | 
| Summarization           | Use to summarize a large amount of text to a short description													| model.summarization     			| 
| Text Generation         | Use to generate text from a preceding example 																					| model.text_generation     		| 
| Translation             | Use for translating one language to another, for example English to French							| model.translation    					|

### Text Classification 

Text classification can be use to generate labels from text. For example, let's say you want to train your own custom model to determine the generes of movies, you run the following query to train a model using specific training data from table `movie` and using the base model `text_classification` for fine tuning.

To prepare the training dataset, we can run the following query to get the the text and labels
```
SELECT movie.overview as text, movie.genre as labels FROM mldb.movie as movie
```

| text      | labels                |
| -------   | -------------------   |
| Two imprisoned men bond over a number of years, finding solace and eventual redemption through acts of common decency.         | Drama                 |
| An organized crime dynasty's aging patriarch transfers control of his clandestine empire to his reluctant son.         | Crime, Drama          |
| When the menace known as the Joker wreaks havoc and chaos on the people of Gotham, Batman must accept one of the greatest psychological and physical tests of his ability to fight injustice.         | Action, Crime, Drama  |


To train a custom model, we can run the following query
```
CREATE MODEL model.my_movie_classification_model
FROM (SELECT movie.overview as text, movie.genre as labels FROM mldb.movie) as training_data
WHERE MODEL_NAME = 'text_classification'
```

model_id      | model_name                        | model_type    | training_data                                                                                             | status        |
-----------	  | --------------------------------  | ----------	  | --------------------------------------------------------------------------------------------------------  | ------------  |
my_model_id	  | movie_genre_classification_model        | text_classification	  | SELECT movie.overview as text, movie.genre as labels FROM mldb.movie  | initializing  |

### Recommender

Recommender can be use to predict items that a user will be interested in base on other users with similar interests. For example, let's say we want recommend movies to our users.  We can use a database table of movie rating to trained our custom model. To get this sample data, import it from [movie rating dataset](/setup/dataset/#movie-rating).

| user_id   | movie_id  | rating  |
| -------   | -------   | ------  |
| 1         | 31        | 2.5     |
| 1         | 1029      | 3       |

To prepare a training dataset with only the column name `user_id` and `item_id` for the model. Movies are rated from 1-5, and since we only need positive rating in our training data, we can run the following query to create the training data for positive reviews higher than 3. To preview the training data we can run the following query
```
SELECT user_id as user_id, movie_id as item_id FROM mldb.movie_rating WHERE mldb.movie_rating.rating > 3
```

| user_id   | item_id   |
| -------   | -------   |
| 1         | 1172      |
| 1         | 1339      |

We can then run the following query to train a model under the name `my_movie_recommender_model` using our training_data. To indicate that this training is for a recommender model, we will need to pass in the condition `MODEL_NAME = 'recommender'`
```
CREATE MODEL model.my_movie_recommender_model
FROM (SELECT user_id as user_id, movie_id as item_id FROM mldb.movie_rating WHERE mldb.movie_rating.rating > 3) as training_data
WHERE MODEL_NAME = 'recommender'
```

model_id      | model_name                        | model_type    | training_data                                                                                             | status        |
-----------	  | --------------------------------  | ----------	  | --------------------------------------------------------------------------------------------------------  | ------------  |
my_model_id	  | my_movie_recommender_model        | recommender	  | SELECT user_id as user_id, movie_id as item_id FROM mldb.movie_rating WHERE mldb.movie_rating.rating > 3  | initializing  |

The result set will provide the `model_id` for our custom model. To make predictions for this model, continue at [predict recommender model](/operations/predict/#recommender).


### Summarization

Summarization can be use to summarize a large amount of text to a short description . For example, let's say you want to train a custom model to summarize the movie description to a few lines, you run the following query to train a model using specific training data from table `movie` and using the base model `summarization` for fine tuning.

```
SELECT movie.overview as summary, movie.description as source FROM mldb.movie
```

| summary      | source                |
| -------      | -------------------   |
| Two imprisoned men bond over a number of years, finding solace and eventual redemption through acts of common decency. | ...... |
| An organized crime dynasty's aging patriarch transfers control of his clandestine empire to his reluctant son.         | ...... |

To train a custom model, we can run the following query
```
CREATE MODEL model.my_movie_summarization_model
FROM (SELECT movie.overview as summary, movie.description as source FROM mldb.movie) as training_data
WHERE MODEL_NAME = 'summarization'
```

model_id      | model_name                        | model_type    | training_data                                                                                             | status        |
-----------	  | --------------------------------  | ----------	  | --------------------------------------------------------------------------------------------------------  | ------------  |
my_model_id	  | my_movie_summarization_model        | summarization	  | SELECT movie.overview as summary, movie.details as source FROM mldb.movie | initializing  |

### Translation

Translation model can be use to translate text from one language to another. For example, let's say you want to train a custom model to do french translation for caption in your movies, you run the following query to train a model using specific training data from table `movie_captions` and using the base model `translation` for fine tuning.

To get the training data, we can run the following query
```
SELECT captions.orignal as source, captions.french as target FROM mldb.movie_captions as captions where language = 'french'
```

| source      | target                |
| -------     | -------------------   |
| Two imprisoned men bond over a number of years, finding solace and eventual redemption through acts of common decency. | Deux hommes emprisonnés se lient pendant plusieurs années, trouvant un réconfort et une éventuelle rédemption grâce à des actes de décence commune. |
| An organized crime dynasty's aging patriarch transfers control of his clandestine empire to his reluctant son.         | Le patriarche vieillissant d'une dynastie du crime organisé transfère le contrôle de son empire clandestin à son fils réticent. |

To train a custom model, we can run the following query
```
CREATE MODEL model.movie_french_translation_model
FROM (SELECT captions.orignal as source, captions.french as target FROM mldb.movie_captions as captions where language = 'french') as training_data
WHERE MODEL_NAME = 'translation'
```

model_id      | model_name                        | model_type    | training_data                                                                                             | status        |
-----------	  | --------------------------------  | ----------	  | --------------------------------------------------------------------------------------------------------  | ------------  |
my_model_id	  | my_movie_translation_model        | translation	  | SELECT captions.orignal as source, captions.french as target FROM mldb.movie_captions as captions where language = 'french' | initializing  |