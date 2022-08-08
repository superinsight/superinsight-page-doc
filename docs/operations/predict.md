
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
SELECT movie._id, predictions.*
FROM mldb.movie as movie
JOIN model.text_classification as m on m.inputs = movie.overview
JOIN model.text_classification as m on m.labels = ['comedy,drama,others']
WHERE mldb.movie._id < 3
```

| _id  | inputs                                     						 				                                                        | _score_comedy  | _score_drama  	| _score_others  | 
| --   | ----------------------------------------------------	 	 					                                                      | -------------- | -------------- | -------------- |
| 1    | Two imprisoned men bond over a number of years, finding solace and eventual redemption through acts of common decency. | 0.0624      	 | 0.7388				  | 0.1987   		   |
| 2    | An organized crime dynasty's aging patriarch transfers control of his clandestine empire to his reluctant son.         | 0.0272         | 0.7299 			  | 0.24282	  		 |


### Question Answering

Question answering can be use to find answers from text. For example, let's say you have the description of a movie and you need to find the main character, you can use question answering model to determine that for you.
```
SELECT movie._id, predictions.inputs, predictions.question, predictions.answer, predictions.score
FROM mldb.movie as movie
JOIN model.question_answering m on m.inputs = movie.overview
JOIN model.question_answering as m on m.question = ['Name of character']
WHERE mldb.movie._id < 20 and predictions.score > 0.4 order by predictions.score
```

| _id       | inputs			  	                                     						| question           | answer    		     | score  |
| --   			| ----------------------------------------------------	 	 				| -----------------	 | --------------    | ------ |
| 11   			| A meek Hobbit from the Shire and eight companions set out on a journey to destroy the powerful One Ring and save Middle-earth from the Dark Lord Sauron. | Name of character  |  Hobbit           | 0.6807  |
| 8   			| In German-occupied Poland during World War II, industrialist Oskar Schindler gradually becomes concerned for his Jewish workforce after witnessing their persecution by the Nazis. | Name of character  |  Oskar Schindler  | 0.2687  |


### Summarization

Summarization can be use to summarize long text to a shorter summary. For example, let's say you need to summary a movie into a short summary, you can use the summarization model to do that for you.
```
SELECT movie._id, movie.title, predictions.*
FROM mldb.movie as movie
JOIN model.summarization m on m.inputs = movie.overview
WHERE mldb.movie._id = 12
```

| _id | title         | overview                             		 	            						| summary            				|
| --  | ---           | ----------------------------------------------------	 	 					| -----------------					|
| 12  | Forrest Gump  | The presidencies of Kennedy and Johnson, the events of Vietnam, Watergate and other historical events unfold through the perspective of an Alabama man with an IQ of 75, whose only desire is to be reunited with his childhood sweetheart.	| An Alabama man with an IQ of 75, whose only desire is to be reunited with his childhood sweetheart.	|



### Text Generation 

Text generation model can be use to create text to get insights from existing data. For example, if we know that a person the movie Forrest Gump, we can use the contents of Forrest Gump and ask the model to predict another movie the user will like.

???+ quote "Generate Movie Recommendation"
    The presidencies of Kennedy and Johnson, the events of Vietnam, Watergate and other historical events unfold through the perspective of an Alabama man with an IQ of 75, whose only desire is to be reunited with his childhood sweetheart.

    Q: If the person likes the above content, which one of the following movie will be this person prefer?

    1. Star Wars
    2. Alien
    3. American Beauty

    A: This person will prefer
```
SELECT movie._id, movie.title, predictions.*
FROM mldb.movie as movie
JOIN model.text_generation m on m.inputs = movie.overview
JOIN model.text_generation m on m.prompt = ['\nQ: If the person likes the above content, which one of the following movie will be this person prefer?\n1.Star Wars\n2.Alien\n3.American Beauty\nA: This person will prefer']
JOIN model.text_generation m on m.min_length = ['10']
JOIN model.text_generation m on m.max_length = ['20']
JOIN model.text_generation m on m.stop_word = ['\n']
WHERE mldb.movie._id = 12
```

| _id       | title         | inputs                         		 	            						| prompt           				| output           				|
| --   			| -----         | ----------------------------------------------------	 	 		| -----------------       | -----------------				|
| 8    			| Forrest Gump  | The presidencies of Kennedy and Johnson, the events of Vietnam, Watergate and other historical events unfold through the perspective of an Alabama man with an IQ of 75, whose only desire is to be reunited with his childhood sweetheart. 	| Q: If the person likes the above content, which one of the following movie will be this person prefer? <br />1. Star Wars<br />2. Alien<br />3. American Beauty<br />A: This person will prefer           				| American Beauty	        |


### Translation

Translation model can be use to translate text from one language to another
```
SELECT movie._id, movie.title, predictions.*
FROM mldb.movie as movie
JOIN model.text_generation m on m.inputs = movie.overview
JOIN model.text_generation m on m.source_language = ['en_XX']
JOIN model.text_generation m on m.target_language = ['fr_XX']
WHERE mldb.movie._id = 12
```

| _id       | description							                                     			| target            	|
| --   			| ----------------------------------------------------	 	 					| -----------------		|
| 8    			| After more than thirty years of service as one of the Navy’s top aviators, Pete “Maverick” Mitchell (Tom Cruise) is where he belongs, pushing the envelope as a courageous test pilot and dodging the advancement in rank that would ground him. When he finds himself training a detachment of Top Gun graduates for a specialized mission the likes of which no living pilot has ever seen, Maverick encounters Lt. Bradley Bradshaw (Miles Teller), call sign: “Rooster,” the son of Maverick’s late friend and Radar Intercept Officer Lt. Nick Bradshaw, aka “Goose”. Facing an uncertain future and confronting the ghosts of his past, Maverick is drawn into a confrontation with his own deepest fears, culminating in a mission that demands the ultimate sacrifice from those who will be chosen to fly it. 	| Après plus de trente ans de service en tant que l'un des meilleurs aviateurs de la Marine, Pete "Maverick" Mitchell (Tom Cruise) est à sa place, repoussant les limites en tant que pilote d'essai courageux et esquivant l'avancement de grade qui le mettrait à la terre. Lorsqu'il se retrouve à former un détachement de diplômés de Top Gun pour une mission spécialisée comme aucun pilote vivant n'a jamais vu, Maverick rencontre le lieutenant Bradley Bradshaw (Miles Teller), indicatif d'appel : "Rooster", le fils du défunt ami de Maverick et l'officier d'interception radar, le lieutenant Nick Bradshaw, alias "Goose". Confronté à un avenir incertain et confronté aux fantômes de son passé, Maverick est entraîné dans une confrontation avec ses propres peurs les plus profondes, aboutissant à une mission qui exige le sacrifice ultime de ceux qui seront choisis pour la piloter.	|