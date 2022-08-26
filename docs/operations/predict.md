
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

Text classification can be use to generate labels from text. For example, let's say you want to determine if movie overview are consider as are comedy, drama or others, and you run the following query 
```
SELECT mldb.movie._id, predictions.*
FROM mldb.movie
JOIN model.text_classification
ON model.text_classification.inputs = mldb.movie.overview
WHERE mldb.movie._id <= 3
AND model.text_classification.labels = 'comedy,drama,others'
```

| _id  | inputs                                     						 				                                    | score_comedy   | score_drama    | score_others    | 
| --   | ----------------------------------------------------	 	 					                                        | -------------- | -------------- | --------------  |
| 1    | Two imprisoned men bond over a number of years, finding solace and eventual redemption through acts of common decency. | 0.0624      	 | 0.7388	      | 0.1987   	    |
| 2    | An organized crime dynasty's aging patriarch transfers control of his clandestine empire to his reluctant son.         | 0.0272         | 0.7299 	      | 0.24282	  	    |
| 3    | When the menace known as the Joker wreaks havoc and chaos on the people of Gotham, Batman must accept one of the greatest psychological and physical tests of his ability to fight injustice. | 0.0120         | 0.8850	      | 0.10289	  	    |

### Question Answering

Question answering can be use to find answers from text. For example, let's say you have the description of a movie and you need to find the main character, you can use question answering model to determine that for you.
```
SELECT mldb.movie._id, predictions.inputs, predictions.question, predictions.answer, predictions.score
FROM mldb.movie
JOIN model.question_answering
ON model.question_answering.inputs = mldb.movie.overview
WHERE mldb.movie._id < 20
AND predictions.score > 0.2
AND model.question_answering.question = 'Name of character'
ORDER BY predictions.score DESC
```

| _id       | inputs			  	                                     						| question           | answer    		     | score  |
| --   			| ----------------------------------------------------	 	 				| -----------------	 | --------------    | ------ |
| 11   			| A meek Hobbit from the Shire and eight companions set out on a journey to destroy the powerful One Ring and save Middle-earth from the Dark Lord Sauron. | Name of character  |  Hobbit           | 0.6807  |
| 8   			| In German-occupied Poland during World War II, industrialist Oskar Schindler gradually becomes concerned for his Jewish workforce after witnessing their persecution by the Nazis. | Name of character  |  Oskar Schindler  | 0.2687  |


### Summarization

Summarization can be use to summarize long text to a shorter summary. For example, let's say you need to summary a movie into a short summary, you can use the summarization model to do that for you.
```
SELECT mldb.movie._id, mldb.movie.title,mldb.movie.overview, predictions.summary
FROM mldb.movie
JOIN model.summarization
ON model.summarization.inputs = mldb.movie.overview
WHERE mldb.movie._id = 12
```

| _id | title         | overview                             		 	            						| summary            				|
| --  | ---           | ----------------------------------------------------	 	 					| -----------------					|
| 12  | Forrest Gump  | The presidencies of Kennedy and Johnson, the events of Vietnam, Watergate and other historical events unfold through the perspective of an Alabama man with an IQ of 75, whose only desire is to be reunited with his childhood sweetheart.	| An Alabama man with an IQ of 75, whose only desire is to be reunited with his childhood sweetheart.	|



### Text Generation 

Text generation model can be use to create text to get insights from existing data. For example, if we know that a person likes certain content, we can use it generate recommendations

???+ quote "Generate Recommendations"
    A thief who steals corporate secrets through the use of dream-sharing technology is given the inverse task of planting an idea into the mind of a C.E.O.

    Q: If the person likes the above content, list other movies this person will like in comma delimeter format?
    
    A: `The Matrix, The Truman Show, The Prestige, Inception, The Dark Knight`
```
SELECT mldb.movie.overview, predictions.prompt, predictions.output
FROM mldb.movie
JOIN model.text_generation
ON model.text_generation.inputs = mldb.movie.overview
WHERE mldb.movie._id = 9
AND model.text_generation.prompt = '\nQ: If the person likes the above content, list other movies this person will like in comma delimeter format?\nA:'
AND model.text_generation.stop_word = '\\n'
```

| overview                         		 	            			| prompt           		| output           				|
| ----------------------------------------------------	 	 		| -----------------     | -----------------				|
| A thief who steals corporate secrets through the use of dream-sharing technology is given the inverse task of planting an idea into the mind of a C.E.O. 	| Q: If the person likes the above content, list other movies this person will like in comma delimeter format? A: | The Matrix, The Truman Show, The Prestige, Inception, The Dark Knight |


### Translation

Translation model can be use to translate text from one language to another.

See the `Supported Language Code` section below for all supported languages.
```
SELECT mldb.movie._id, mldb.movie.title, predictions.inputs, predictions.output
FROM mldb.movie
JOIN model.translation
ON model.translation.inputs = mldb.movie.overview
WHERE mldb.movie._id = 12
AND model.translation.source_language = ['en_XX']
AND model.translation.target_language = ['fr_XX']
```

| _id       | title         | inputs                         		 	            			| output           				|
| --   		| -----         | ----------------------------------------------------	 	 		| -----------------				|
| 8    		| Forrest Gump  | The presidencies of Kennedy and Johnson, the events of Vietnam, Watergate and other historical events unfold through the perspective of an Alabama man with an IQ of 75, whose only desire is to be reunited with his childhood sweetheart. | Les présidences de Kennedy et Johnson, les événements du Vietnam, Watergate et d'autres événements historiques se déroulent dans la perspective d'un homme d'Alabama ayant un IQ de 75 ans, dont le seul désir est de se réunir avec son ami de l'enfance. |

???+ quote "Supported Language Code"

    Arabic (ar_AR), Czech (cs_CZ), German (de_DE), English (en_XX), Spanish (es_XX), Estonian (et_EE), Finnish (fi_FI), French (fr_XX), Gujarati (gu_IN), Hindi (hi_IN), Italian (it_IT), Japanese (ja_XX), Kazakh (kk_KZ), Korean (ko_KR), Lithuanian (lt_LT), Latvian (lv_LV), Burmese (my_MM), Nepali (ne_NP), Dutch (nl_XX), Romanian (ro_RO), Russian (ru_RU), Sinhala (si_LK), Turkish (tr_TR), Vietnamese (vi_VN), Chinese (zh_CN), Afrikaans (af_ZA), Azerbaijani (az_AZ), Bengali (bn_IN), Persian (fa_IR), Hebrew (he_IL), Croatian (hr_HR), Indonesian (id_ID), Georgian (ka_GE), Khmer (km_KH), Macedonian (mk_MK), Malayalam (ml_IN), Mongolian (mn_MN), Marathi (mr_IN), Polish (pl_PL), Pashto (ps_AF), Portuguese (pt_XX), Swedish (sv_SE), Swahili (sw_KE), Tamil (ta_IN), Telugu (te_IN), Thai (th_TH), Tagalog (tl_XX), Ukrainian (uk_UA), Urdu (ur_PK), Xhosa (xh_ZA), Galician (gl_ES), Slovene (sl_SI)
