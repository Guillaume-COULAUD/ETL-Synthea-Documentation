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
⚠️ If it can't find the repository, you need to set your proxy in R.
library(ETLSyntheaBuilder)
⚠️ When it has ask to you to make a choice, choose 1.
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

⚠️ If you have some insert issues, use a synthea version compatible with your Synthea files!!
```

```
Schema to load the Synthea tables :

syntheaSchema <- "native"

⚠️ You need to create this schema in your database. You can name it like you want.
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

Disable triggers

ALTER TABLE omop_cdm.care_site DISABLE TRIGGER ALL;
ALTER TABLE omop_cdm.cdm_source DISABLE TRIGGER ALL;
ALTER TABLE omop_cdm.cohort DISABLE TRIGGER ALL;
ALTER TABLE omop_cdm.cohort_definition DISABLE TRIGGER ALL;
ALTER TABLE omop_cdm.concept DISABLE TRIGGER ALL;
ALTER TABLE omop_cdm.concept_ancestor DISABLE TRIGGER ALL;
ALTER TABLE omop_cdm.concept_class DISABLE TRIGGER ALL;
ALTER TABLE omop_cdm.concept_relationship DISABLE TRIGGER ALL;
ALTER TABLE omop_cdm.concept_synonym DISABLE TRIGGER ALL;
ALTER TABLE omop_cdm.condition_era DISABLE TRIGGER ALL;
ALTER TABLE omop_cdm.condition_occurrence DISABLE TRIGGER ALL;
ALTER TABLE omop_cdm.cost DISABLE TRIGGER ALL;
ALTER TABLE omop_cdm.death DISABLE TRIGGER ALL;
ALTER TABLE omop_cdm.device_exposure DISABLE TRIGGER ALL;
ALTER TABLE omop_cdm.domain DISABLE TRIGGER ALL;
ALTER TABLE omop_cdm.dose_era DISABLE TRIGGER ALL;
ALTER TABLE omop_cdm.drug_era DISABLE TRIGGER ALL;
ALTER TABLE omop_cdm.drug_exposure DISABLE TRIGGER ALL;
ALTER TABLE omop_cdm.drug_strength DISABLE TRIGGER ALL;
ALTER TABLE omop_cdm.episode DISABLE TRIGGER ALL;
ALTER TABLE omop_cdm.episode_event DISABLE TRIGGER ALL;
ALTER TABLE omop_cdm.fact_relationship DISABLE TRIGGER ALL;
ALTER TABLE omop_cdm.location DISABLE TRIGGER ALL;
ALTER TABLE omop_cdm.measurement DISABLE TRIGGER ALL;
ALTER TABLE omop_cdm.metadata DISABLE TRIGGER ALL;
ALTER TABLE omop_cdm.note DISABLE TRIGGER ALL;
ALTER TABLE omop_cdm.note_nlp DISABLE TRIGGER ALL;
ALTER TABLE omop_cdm.observation DISABLE TRIGGER ALL;
ALTER TABLE omop_cdm.observation_period DISABLE TRIGGER ALL;
ALTER TABLE omop_cdm.payer_plan_period DISABLE TRIGGER ALL;
ALTER TABLE omop_cdm.person DISABLE TRIGGER ALL;
ALTER TABLE omop_cdm.procedure_occurrence DISABLE TRIGGER ALL;
ALTER TABLE omop_cdm.provider DISABLE TRIGGER ALL;
ALTER TABLE omop_cdm.relationship DISABLE TRIGGER ALL;
ALTER TABLE omop_cdm.source_to_concept_map DISABLE TRIGGER ALL;
ALTER TABLE omop_cdm.specimen DISABLE TRIGGER ALL;
ALTER TABLE omop_cdm.visit_detail DISABLE TRIGGER ALL;
ALTER TABLE omop_cdm.visit_occurrence DISABLE TRIGGER ALL;
ALTER TABLE omop_cdm.vocabulary DISABLE TRIGGER ALL;

```

```

Load the vocabulary from your Athena files

ETLSyntheaBuilder::LoadVocabFromCsv(connectionDetails = cd, cdmSchema = cdmSchema, vocabFileLoc = vocabFileLoc)

```

```

ETLSyntheaBuilder::CreateMapAndRollupTables(connectionDetails = cd, cdmSchema = cdmSchema, syntheaSchema = syntheaSchema, cdmVersion = cdmVersion, syntheaVersion = syntheaVersion)

```

```

ETLSyntheaBuilder::CreateExtraIndices(connectionDetails = cd, cdmSchema = cdmSchema, syntheaSchema = syntheaSchema, syntheaVersion = syntheaVersion)

```

Before start the next command, on your cdm : 
 - Table measurement : Change all 50 strings limit by 100
 - payer_plan_period : allow nulle value for payer_plan_period_end_date

```

ETLSyntheaBuilder::LoadEventTables(connectionDetails = cd, cdmSchema = cdmSchema, syntheaSchema = syntheaSchema, cdmVersion = cdmVersion, syntheaVersion = syntheaVersion)

```

```

Enable triggers

ALTER TABLE omop_cdm.care_site ENABLE TRIGGER ALL;
ALTER TABLE omop_cdm.cdm_source ENABLE TRIGGER ALL;
ALTER TABLE omop_cdm.cohort ENABLE TRIGGER ALL;
ALTER TABLE omop_cdm.cohort_definition ENABLE TRIGGER ALL;
ALTER TABLE omop_cdm.concept ENABLE TRIGGER ALL;
ALTER TABLE omop_cdm.concept_ancestor ENABLE TRIGGER ALL;
ALTER TABLE omop_cdm.concept_class ENABLE TRIGGER ALL;
ALTER TABLE omop_cdm.concept_relationship ENABLE TRIGGER ALL;
ALTER TABLE omop_cdm.concept_synonym ENABLE TRIGGER ALL;
ALTER TABLE omop_cdm.condition_era ENABLE TRIGGER ALL;
ALTER TABLE omop_cdm.condition_occurrence ENABLE TRIGGER ALL;
ALTER TABLE omop_cdm.cost ENABLE TRIGGER ALL;
ALTER TABLE omop_cdm.death ENABLE TRIGGER ALL;
ALTER TABLE omop_cdm.device_exposure ENABLE TRIGGER ALL;
ALTER TABLE omop_cdm.domain ENABLE TRIGGER ALL;
ALTER TABLE omop_cdm.dose_era ENABLE TRIGGER ALL;
ALTER TABLE omop_cdm.drug_era ENABLE TRIGGER ALL;
ALTER TABLE omop_cdm.drug_exposure ENABLE TRIGGER ALL;
ALTER TABLE omop_cdm.drug_strength ENABLE TRIGGER ALL;
ALTER TABLE omop_cdm.episode ENABLE TRIGGER ALL;
ALTER TABLE omop_cdm.episode_event ENABLE TRIGGER ALL;
ALTER TABLE omop_cdm.fact_relationship ENABLE TRIGGER ALL;
ALTER TABLE omop_cdm.location ENABLE TRIGGER ALL;
ALTER TABLE omop_cdm.measurement ENABLE TRIGGER ALL;
ALTER TABLE omop_cdm.metadata ENABLE TRIGGER ALL;
ALTER TABLE omop_cdm.note ENABLE TRIGGER ALL;
ALTER TABLE omop_cdm.note_nlp ENABLE TRIGGER ALL;
ALTER TABLE omop_cdm.observation ENABLE TRIGGER ALL;
ALTER TABLE omop_cdm.observation_period ENABLE TRIGGER ALL;
ALTER TABLE omop_cdm.payer_plan_period ENABLE TRIGGER ALL;
ALTER TABLE omop_cdm.person ENABLE TRIGGER ALL;
ALTER TABLE omop_cdm.procedure_occurrence ENABLE TRIGGER ALL;
ALTER TABLE omop_cdm.provider ENABLE TRIGGER ALL;
ALTER TABLE omop_cdm.relationship ENABLE TRIGGER ALL;
ALTER TABLE omop_cdm.source_to_concept_map ENABLE TRIGGER ALL;
ALTER TABLE omop_cdm.specimen ENABLE TRIGGER ALL;
ALTER TABLE omop_cdm.visit_detail ENABLE TRIGGER ALL;
ALTER TABLE omop_cdm.visit_occurrence ENABLE TRIGGER ALL;
ALTER TABLE omop_cdm.vocabulary ENABLE TRIGGER ALL;

```

## Usefull links

[ETL-Synthea](https://github.com/OHDSI/ETL-Synthea)

[Synthea](https://synthetichealth.github.io/synthea/)
