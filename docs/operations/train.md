
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

Text classification can be use to generate labels from text. For example, let's say you want to determine if movie reviews are positive or negative, and you run the following query 
```
CREATE MODEL demo_db.movie_genre_classification_model
FROM (SELECT movie.description as text, movie.genere as labels FROM demo_db.my_database.movie as movie) as text_classification
```