
## Train custom models
To train your own custom model, you can choose a pretrained ML model as a based model, and finetuned it with your own data

| Task            				| Description                                                         										| Model Name 										|
| ----------------------- | -----------------------------------------------                       									| ----------------------------- |
| Text Classification     | Use to assign label or class to text, labels such as sentiment or product category			| model.text_classification    | 
| Question Answering      | Use to find answers in a large amount of text    																			  | model.question_answering | 
| Summarization           | Use to summarize a large amount of text to a short description													| model.summarization     			| 
| Text Generation         | Use to generate text from a preceding example 																					| model.text_generation     		| 
| Translation             | Use for translating one language to another, for example English to French							| model.translation    					|

### Text Classification 

Text classification can be use to generate labels from text. For example, let's say you want to train your own custom model to determine if movie reviews are positive or negative, you run the following query to train a model using specific training data from table `movie` and using the base model `text_classification` for fine tuning.
```
CREATE MODEL model.movie_genre_classification_model
FROM (SELECT movie.description as text, movie.genere as labels FROM mldb.movie as movie) as training_data
WHERE MODEL_NAME = 'text_classification'
```

### Summarization

Translation model can be use to translate text from one language to another. For example, let's say you want to train a custom model to summarize the movie description to a few headlines, you run the following query to train a model using specific training data from table `movie` and using the base model `summarization` for fine tuning.
```
CREATE MODEL model.movie_headline_summarization_model
FROM (SELECT movie.description as source, movie.headline as summary FROM mldb.movie as movie) as training_data
WHERE MODEL_NAME = 'summarization'
```

### Translation

Translation model can be use to translate text from one language to another. For example, let's say you want to train a custom model to do french translation for caption in your movies, you run the following query to train a model using specific training data from table `movie_captions` and using the base model `translation` for fine tuning.
```
CREATE MODEL model.movie_french_translation_model
FROM (SELECT captions.orignal as source, captions.french as target FROM mldb.movie_captions as captions where language = 'french') as training_data
WHERE MODEL_NAME = 'translation'
```