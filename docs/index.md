Superinsight is a Machine Learning Database.
Traditional database are execellent at searching data types such as text and numbers, however it does not support for machine learning. Data is the most crucial part of machine learning, but because traditional databases do not support machine learning operations, for any ML operations today, data need to move outside the database for training, inference and search, it made machine learning a lot more difficult that it has to be. Also the tools we use today for machine learning such as Pytorch and Tensorflow are made for data scientist and ML engineers, these are not tools that developers and database administrators are familar with. We think the better approach to this problem is to redesign the process by keeping all the machine learning operations inside the database where the data is. 

## What does Machine Learning Database provide?

### 1. Semantic Search
Traditional database tables only allow users to search text and numbers using keyword matching or full text search. By default, machine learning database tables store text as embeddings, making it easy for users to perfrom semantic search with simple SQL statement.


### 2. Inference 
Machine learning database has build in ML models that allow users to make inference as part of the database operations. Common ML operations such as classification, summarization, text generation and translation are available by default for users to make inference on these models with their data. Inference can also be perform using simple SQL statement.

### 3. Training 
Machine learning database not only provide default models, it also allow users to use their existing data to train or finetuned ML models. Training models can also be perform using simple SQL statment.


### 4. Federated Query 
It is very common that the data you need for your ML operations are located from other external data sources. Superinsight ML database also come with the Federated Query feature, which will allow you to query data from multiple data sources using one sql query. This will elimiate the need of building out a ETL pipeline, which takes a lot of time and the possiblity of data inconsistency.

## Who is it for?
The purpose of Superinsight is to accelerate the adoption of AI for every organization in the world by empowering organization domain experts to utilize AI using their existing skills and tools. While there are many different types of domain experts in each organization, Superinsight is made for data experts, business experts and application experts.


### 1. Data Experts
Data experts are usually people in the organization who are database admins, database developers or those who understand and has access to the data. We all know that data is the most critial part of AI, and data experts have the most knowledge of where the data is and how data are being structured within the organization. Today one of the main reason organizations are being held back on AI adoption is because the tools that we use for machine learning such as PyTorch and Tensorflow are made of AI researchers and ML engineers. Most data experts do not come from a machine learning background, and most organizations do not have a team of AI researchers and ML engineers who understand the data structure as well as the data experts. To overcome this barrier, we must enable data experts to use their existing skills to perform AI functions. SQL has been and still is the most dominant tools data experts. If we enable data experts to use SQL to query data across different data sources and empower them to train models, inference models, generate and store data without becoming a machine learning experts, then data experts will be able to ignite AI adoptions across the entire organization.

Superinsight has created the concept of a ML database. Just like a traditional database that is use to store and retrieve the typical datatypes, ML database will also index data in such a way that it can be used for AI functions. Superinsight also provide a SQL query engine that will help data experts to transform data from existing databases to ML database. Data experts will be able to use SQL to run ML predictions by query data from ML tables and joining data from other data sources such as SQL or NoSQL databases in a single SQL statement. Superinsight will empower data experts to provide the foundation they need to help their organization adopt AI.

### 2. Business Experts
Business experts are usually people in the organization who are executives, business analysts, sales, marketers or those who need to get insights from the data within the organization. We all know that main benefit of AI is to be able to perform human like judgement at scale. Another main reason organizations are being held back on AI adoption is because the tools that we use to get AI insights such as Python, R, Jupyter Notebooks, MATLAB and Scikit-learn are made of data scientists. Even though in recent years, there is a huge movement to train more data scientists, the barrier to entry is still too high. Most business experts are not equipped with skills of a data scientist, and most data scientists are not easier to hire or train. To overcome this barrier, we must enable business experts to use their existing skills to get AI insights.

Today most business experts use data visualization tools such as Tableau, Power BI, Sisense and Superset to get insights from their data, and most of these BI tools use SQL for data retrieval. Since Superinsight provides a SQL Query Engine can be use to make ML predictions, business experts can use it to connect their existing BI tools.  With this new approach, business experts will get AI insights in real time and be more informed about their business so they make better decisions faster.

### 3. Developers
Developers are the people who build applications for people inside and outside the organization. Application is the main component that is use for capturing real world information and store them as data. Every developer would love to build applications with AI capabilities, but most of them are being held back because most of the tools used in machine learning development such as Pytorch and Tensorflow are made for ML engineers. Most organizations do not have ML engineers as part of the development team, and ML engineers are hard to hire and train. To overcome this barrier, we must enable developers to incorporate AI features into their applications without the need of becoming ML engineers.

Since Superinsight provides a SQL Query Engine for developers to perform ML predictions from their ML database using SQL query. With this new approach, developers can focus on builing features and incorporate AI functions for their users inside and outside of the organization, helping their organization moving towards AI adoption.