# ETL-Synthea 

ETL-Syntea Allows you to load data from Synthea files in your OMOP CDM, but it also allows you to load vocabulary from Athena files in your CDM. 

## Prerequisites
 - [R](https://cran.r-project.org/bin/windows/base/)
 - [OMOP CDM](https://github.com/OHDSI/CommonDataModel)
 - [Athena vocabulary files](https://athena.ohdsi.org/search-terms/start)


## 1.Download Synthea file

You can download already generate files [here](https://synthea.mitre.org/downloads)

## 2. Install and use ETL-Synthea with R

#### Install ETL-Synthea


``` 
devtools::install_github("OHDSI/ETL-Synthea")
library(ETLSyntheaBuilder)
```

#### Define database connection

```
cd <- DatabaseConnector::createConnectionDetails(
  dbms     = "postgresql", 
  server   = "localhost/postgres", 
  user     = "postgres", 
  password = "mypass", 
  port     = 5432, 
  pathToDriver = "c:/drivers"  
)
```

- dbms : Database management system (Postgresql, Oracle...)
- server : {host}\\{data base name}
- user : User of your database with enough rights for the next steps
- password : Password of your user
- port : Database connection port
- pathToDriver : Database driver path

#### Create variables

```
CDM schema name :

cdmSchema <- "omop_cdm" 

```


```
CDM version :

cdmVersion <- "5.4"

```

```
Synthea version :

syntheaVersion <- "3.2.0"

⚠️ If you have some insert issues, use a more recent synthea version!!
```

```
Schema to load the Synthea tables :

syntheaSchema <- "native"

⚠️ Don't need to be modify
```

```
Path to the folder where your Synthea files, previously downloaded, are :

syntheaFileLoc <- " c:\\download\\synthea"

```


```
Path to the folder where your Athena files, previously downloaded, are :

vocabFileLoc   <- "c:\\download\\Athena"

```


#### Use Synthea

```
Create Synthea tables

ETLSyntheaBuilder::CreateSyntheaTables(connectionDetails = cd, syntheaSchema = syntheaSchema, syntheaVersion = syntheaVersion)

```


```

Load Synthea tables

ETLSyntheaBuilder::LoadSyntheaTables(connectionDetails = cd, syntheaSchema = syntheaSchema, syntheaFileLoc = syntheaFileLoc)

```


```

Load the vocabulary from your Athena files

ETLSyntheaBuilder::LoadVocabFromCsv(connectionDetails = cd, cdmSchema = cdmSchema, vocabFileLoc = vocabFileLoc)


```

```

ETLSyntheaBuilder::CreateMapAndRollupTables(connectionDetails = cd, cdmSchema = cdmSchema, syntheaSchema = syntheaSchema, cdmVersion = cdmVersion, syntheaVersion = syntheaVersion)


```


## Usefull links

[ETL-Synthea](https://github.com/OHDSI/ETL-Synthea)

[Synthea](https://synthetichealth.github.io/synthea/)
