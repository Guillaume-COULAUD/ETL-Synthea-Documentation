# Broadsea WebAPI

Broadsea WebAPI run on Docker and allows to use OHDSI Atlas, OHDSI Ares and OHDSI Hades. You can run it with your own database and your own ad. But if you don't have omop cdm, WebAPI supplis data example.

## Prerequisites
 - [Docker](https://docs.docker.com/get-docker/)
 - [OMOP CDM](https://github.com/OHDSI/CommonDataModel)
 - [Achilles](https://github.com/Guillaume-COULAUD/ETL-Synthea-Documentation/blob/main/Achilles-documentation.md)


## 1. Clone Broadsea repository

```
git clone https://github.com/OHDSI/Broadsea.git
```

## 2. Edit .env file

In .env file edit section 3, 7, 9 and 10 for connect WebAPI to your database. 

In section 3 you need to provide a schema booked for the WebAPI. If it doesn't exist, it will be automatically created.
In section 7, 9 and 10, you need to provide your omop cmd schema (you can provide others schemas if you want, but for this example use your omop cdm schema).
If you want, you can edit others sections to connect WebAPI to your AD...

## 3. Start WebAPI

Run the following command, in the folder where your docker-compose.yml file is present.

```
docker compose pull && docker-compose --profile default up -d

If you want to see more option you can visit [Broadsea repository](https://github.com/OHDSI/Broadsea/tree/main)
```

When your WebAPI is running, go on http//{your host}/WebAPI/ddl/results?dialect={your dmbs}&schema={your result schema name}&vocabSchema={your vocab schema name}&tempSchema={your temp schema name}&initConcepthierarchy=true

For example : 
http://127.0.0.1/WebAPI/ddl/results?dialect=postgresql&schema=results&vocabSchema=omop_cdm&tempSchema=temp&initConcepthierarchy=true

Copy and pasted the queries on the web page in your dmbs and execute them.

After that execute the following queries to insert Data sources: 

```

INSERT INTO webapi.source (source_id, source_name, source_key, source_connection, source_dialect) 
SELECT nextval('webapi.source_sequence'), 'My Cdm', 'MY_CDM', ' jdbc:postgresql://{your host}:5432/cdm?user={user}&password={password}', '{your dmbs}';

INSERT INTO webapi.source_daimon (source_daimon_id, source_id, daimon_type, table_qualifier, priority) 
SELECT nextval('webapi.source_daimon_sequence'), source_id, 0, '{your omop cdm}', 0
FROM webapi.source
WHERE source_key = 'MY_CDM'
;

INSERT INTO webapi.source_daimon (source_daimon_id, source_id, daimon_type, table_qualifier, priority) 
SELECT nextval('webapi.source_daimon_sequence'), source_id, 1, '{your omop cdm (or vocab cdm if your vocab is not in your omop cdm)}', 1
FROM webapi.source
WHERE source_key = 'MY_CDM'
;

INSERT INTO webapi.source_daimon (source_daimon_id, source_id, daimon_type, table_qualifier, priority) 
SELECT nextval('webapi.source_daimon_sequence'), source_id, 2, '{your result cdm}', 1
FROM webapi.source
WHERE source_key = 'MY_CDM'
;

INSERT INTO webapi.source_daimon (source_daimon_id, source_id, daimon_type, table_qualifier, priority) 
SELECT nextval('webapi.source_daimon_sequence'), source_id, 5, '{your temp cdm}', 0
FROM webapi.source
WHERE source_key = 'MY_CDM'
;

```

After, go on url : http://{your host}/WebAPI/source/refresh 

You can see your WebAPI data sources on http://{your host}/WebAPI/source/sources





