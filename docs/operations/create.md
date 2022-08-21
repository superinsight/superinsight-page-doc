# Create Table

Database table can be created using the `CREATE TABLE` statement in superinsight. For the columns that you will like to enable semantic search, assign them with the data type `TEXT`.

```
CREATE TABLE mldb.table_name (
	_id serial PRIMARY KEY,
	column1 TEXT,
	column2 float8,
	column3 varchar(100),
	column4 int4
);
```