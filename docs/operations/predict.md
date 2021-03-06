
## Predict with pretrained models
To make a prediction, you can simply use pretrained ML model by simply utilizing as a table under the model schema. By default ML database comes with the following ML models 

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
SELECT movie.id, movie.movie_id, movie.review, m.labels, m.positive_score, m.negative_score
FROM demo_db.my_database.movie_reviews as movie
JOIN demo_db.model.text_classification as m 
WHERE m.labels = 'positive, negative'
AND movie.movie_id > 2;
```

| id   | movie_id  | review                                     						 					| labels            	| positive_score | negative_score | 
| --   | --------  | ----------------------------------------------------	 	 					| -----------------		| -------------- | -------------- |
| 3    | 2				 | This movie is awesome, it is probably the best movie of the year | positive, negative	| 0.9951				 | 0.0049 	  		|
| 4    | 3				 | This is the worst movie I ever saw, it's not good       					| positive, negative	| 0.0022				 | 0.9978 	  		|


### Question Answering

Question answering can be use to find answers from text. For example, let's say you have the description of a movie and you need to find the main character, you can use question answering model to determine that for you.
```
SELECT movie.movie_id, movie.description, m.answer, m._score 
FROM demo_db.my_database.movie as movie
JOIN demo_db.model.question_answering m
WHERE m.question = 'Who is the main character of the movie'
AND movie.movie_id = 8;
```

| movie_id  | movie							                                     						| answer            				| _score 				 |
| --   			| ----------------------------------------------------	 	 					| -----------------					| -------------- |
| 8    			| After more than thirty years of service as one of the Navy???s top aviators, Pete ???Maverick??? Mitchell (Tom Cruise) is where he belongs, pushing the envelope as a courageous test pilot and dodging the advancement in rank that would ground him. When he finds himself training a detachment of Top Gun graduates for a specialized mission the likes of which no living pilot has ever seen, Maverick encounters Lt. Bradley Bradshaw (Miles Teller), call sign: ???Rooster,??? the son of Maverick???s late friend and Radar Intercept Officer Lt. Nick Bradshaw, aka ???Goose???. Facing an uncertain future and confronting the ghosts of his past, Maverick is drawn into a confrontation with his own deepest fears, culminating in a mission that demands the ultimate sacrifice from those who will be chosen to fly it. 	| Pete ???Maverick??? Mitchell	| 0.5555				 |


### Summarization

Summarization can be use to summarize long text to a shorter summary. For example, let's say you need to summary a movie into a short summary, you can use the summarization model to do that for you.
```
SELECT movie.movie_id, movie.description, m.summary
FROM demo_db.my_database.movie as movie
JOIN demo_db.model.summarization m
WHERE movie.movie_id = 8;
```

| movie_id  | description                         		 	            						| summary            				|
| --   			| ----------------------------------------------------	 	 					| -----------------					|
| 8    			| After more than thirty years of service as one of the Navy???s top aviators, Pete ???Maverick??? Mitchell (Tom Cruise) is where he belongs, pushing the envelope as a courageous test pilot and dodging the advancement in rank that would ground him. When he finds himself training a detachment of Top Gun graduates for a specialized mission the likes of which no living pilot has ever seen, Maverick encounters Lt. Bradley Bradshaw (Miles Teller), call sign: ???Rooster,??? the son of Maverick???s late friend and Radar Intercept Officer Lt. Nick Bradshaw, aka ???Goose???. Facing an uncertain future and confronting the ghosts of his past, Maverick is drawn into a confrontation with his own deepest fears, culminating in a mission that demands the ultimate sacrifice from those who will be chosen to fly it. 	| Pete ???Maverick??? Mitchell (Tom Cruise) is where he belongs, pushing the envelope as a courageous test pilot. Maverick is drawn into a confrontation with his own deepest fears, culminating in a mission that demands the ultimate sacrifice from those who will be chosen to fly it.	|



### Text Generation 

Text generation model can be use to create text to get insights from existing data. For example , let's say you have the description of the movie and you will like to find out what type of audiences are best for the movie. To generate new text, you can submit a prompt to the text generation model. In the following example, we take the summary of the movie and append the text `What type of audience will like movie? This movie is best for people who like` to encourage the model to give us the answer we are looking for.
```
SELECT movie.movie_id, m.prompt, m.text
FROM demo_db.my_database.movie as movie
JOIN demo_db.model.text_generation m
WHERE m.prompt = movie.summary + 'What type of audience will like movie? This movie is best for people who like'
WHERE movie.movie_id = 8;
```

| movie_id  | prompt                         		 	            						| text            				|
| --   			| ----------------------------------------------------	 	 		| -----------------				|
| 8    			| Pete ???Maverick??? Mitchell (Tom Cruise) is where he belongs, pushing the envelope as a courageous test pilot. Maverick is drawn into a confrontation with his own deepest fears, culminating in a mission that demands the ultimate sacrifice from those who will be chosen to fly it. `What type of audience will like movie? This movie is best for people who like`  	| drama and action movies	|


### Translation

Translation model can be use to translate text from one language to another
```
SELECT movie.movie_id, movie.description, m.target
FROM demo_db.my_database.movie as movie
JOIN demo_db.model.translation m
WHERE m.source = m.description and m.source_language = 'en_XX' and m.target_language = 'fr_XX'
AND movie.movie_id = 8;
```

| movie_id  | description							                                     			| target            	|
| --   			| ----------------------------------------------------	 	 					| -----------------		|
| 8    			| After more than thirty years of service as one of the Navy???s top aviators, Pete ???Maverick??? Mitchell (Tom Cruise) is where he belongs, pushing the envelope as a courageous test pilot and dodging the advancement in rank that would ground him. When he finds himself training a detachment of Top Gun graduates for a specialized mission the likes of which no living pilot has ever seen, Maverick encounters Lt. Bradley Bradshaw (Miles Teller), call sign: ???Rooster,??? the son of Maverick???s late friend and Radar Intercept Officer Lt. Nick Bradshaw, aka ???Goose???. Facing an uncertain future and confronting the ghosts of his past, Maverick is drawn into a confrontation with his own deepest fears, culminating in a mission that demands the ultimate sacrifice from those who will be chosen to fly it. 	| Apr??s plus de trente ans de service en tant que l'un des meilleurs aviateurs de la Marine, Pete "Maverick" Mitchell (Tom Cruise) est ?? sa place, repoussant les limites en tant que pilote d'essai courageux et esquivant l'avancement de grade qui le mettrait ?? la terre. Lorsqu'il se retrouve ?? former un d??tachement de dipl??m??s de Top Gun pour une mission sp??cialis??e comme aucun pilote vivant n'a jamais vu, Maverick rencontre le lieutenant Bradley Bradshaw (Miles Teller), indicatif d'appel : "Rooster", le fils du d??funt ami de Maverick et l'officier d'interception radar, le lieutenant Nick Bradshaw, alias "Goose". Confront?? ?? un avenir incertain et confront?? aux fant??mes de son pass??, Maverick est entra??n?? dans une confrontation avec ses propres peurs les plus profondes, aboutissant ?? une mission qui exige le sacrifice ultime de ceux qui seront choisis pour la piloter.	|