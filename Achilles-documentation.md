# Achilles

Achille allow you to create some tables required for OHDSI WebAPI.  

## Prerequisites
 - [R](https://cran.r-project.org/bin/windows/base/)
 - [OMOP CDM](https://github.com/OHDSI/CommonDataModel)
 - [ETL-Synthea](https://github.com/Guillaume-COULAUD/ETL-Synthea-Documentation/blob/main/README.md)

## Install and use Achilles

#### Install Achilles

```

remotes::install_github("OHDSI/Achilles")
library(Achilles)

```

⚠️ If it can't find the repository, you need to set your proxy in R.

#### Create schema

Create two schema "results" and "temp" in your database.

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

#### Use Achilles

```

achillesResults <-Achilles::achilles(  connectionDetails = connectionDetails, cdmDatabaseSchema = "omop_cdm",  resultsDatabaseSchema = "results",  vocabDatabaseSchema = "omop_cdm",  tempEmulationSchema = "temp", scratchDatabaseSchema  = "omop_cdm", createTable = TRUE, cdmVersion = "5.4", outputFolder = "c:\\temp\\")

```

connectionDetails : Database connection information
cdmDatabaseSchama : Name of your OMOP schema in your database
resultsDatabaseSchema : Name of your results schema in your database 
vocabDatabaseSchema : Name of your vocab schema in your database 
tempEmulationSchema : Name of your temp schema in your database 
scratchDatabaseSchema : Name of your scratch schema in your database 
createTable : True or false, if you want Achilles create table
cdmVersion : Version of your CDM
outputFolder : Path folder for store logs and errors from Achilles

⚠️If you have this error : 
⚠️Error in `.createErrorReport()`:
⚠️! Error executing SQL:
⚠️org.postgresql.util.PSQLException: ERREUR: la relation « results.achilles_results » n'existe pas
⚠️You need to look in file "achillesError_2004.txt" in the path you advice in tempEmulationSchema. This error can be caused by many reasons. 

⚠️If you have this error :
⚠️Error:
⚠️org.postgresql.util.PSQLException: ERREUR: division par zéro
⚠️It's because your Person table is empty. Use [ETL-Synthea.](https://github.com/Guillaume-COULAUD/ETL-Synthea-Documentation/blob/main/README.md)


