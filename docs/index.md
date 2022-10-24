Superinsight is a Machine Learning SQL Database, its main purpose is to provide machine learning features within the database so any applications and business intelligence software that utliize SQL can take advantage of AI capabilities.

Traditional database are excellent at searching data types such as text and numbers, however there are no support for machine learning operations. Data is the most crucial part of machine learning, but because traditional databases do not support machine learning operations, data need to move outside the database for training, prediction and search, making machine learning programming far more complicated than it needs to be.

Previously, all the tools build for machine learning are made for AI researchers and data scientists, and not developers. We think the better approach to this problem is utilize the database as an abstraction layer for machine learning, and enable developers to build AI enabled applications at scale.

## What does it do

### 1. Semantic Search
Traditional database tables only allow users to search text and numbers using keyword matching or full text search. By default, machine learning SQL database tables store text and image as embeddings, making it easy for users to perfrom semantic search with simple SQL statement.


### 2. Model Prediction
Machine learning SQL database has build in ML models that allow users to make predictions by joining the models in a SQL query statement, similar to traditional database table. Common ML downstream tasks such as classification, recommendation, summarization, text generation and translation are available by default for users to make predictions using SQL query inside the database.

### 3. Model Training
Machine learning SQL database also provide the ability to train models from scratch or using a pretrained model with data in the database. Training models can also be perform using simple SQL statment.
