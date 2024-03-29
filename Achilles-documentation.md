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
⚠️ If it can't find the repository, you need to set your proxy in R.
library(Achilles)

```

#### Create schema

Create two schema "results" and "temp".

#### Use Achilles

```

achillesResults <-Achilles::achilles(  connectionDetails = **connectionDetails**, cdmDatabaseSchema = **"omop_cdm"**,  resultsDatabaseSchema = "results",  vocabDatabaseSchema = "omop_cdm",  tempEmulationSchema = "temp", scratchDatabaseSchema  = "omop_cdm", createTable = TRUE, cdmVersion = "5.4", outputFolder = "c:\\temp\\")



⚠️If you have this error : 
⚠️Error in `.createErrorReport()`:
⚠️! Error executing SQL:
⚠️org.postgresql.util.PSQLException: ERREUR: la relation « results.achilles_results » n'existe pas
⚠️ You need to look in file "achillesError_2004.txt" in the path you advice in tempEmulationSchema. This error can be caused by many reasons. 

```

