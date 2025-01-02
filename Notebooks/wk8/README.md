# Environmental Rights Violations Chatbot

## Current functionality

Chatbot that queries SQL database containing mean, max, min, median, and standard deviation of air quality by country 
(and by extension, continent) with reasonable but inconsistent accuracy.

For example, some user questions that the chatbot may answer are: 
1. What is the average and standard deviation of air quality in Indonesia?
2. Which five countries have the worst air quality?
3. What is the average air quality on each continent?
4. In how many countries did maximum air quality surpass 100 micrograms per cubic meter?
5. What is the average air quality among countries that start with the letter D?

## Data sources

Data                     | Source                                | Path (`data/input/...`)                     | Description                                                              | Units      | Sp. Res. | Temp. Res.
-------------------------|---------------------------------------|---------------------------------------------|--------------------------------------------------------------------------|------------|----------| --------- 
Global Air Quality       | Naia Ormaza Zulueta <br> [WUSTL](https://sites.wustl.edu/acag/datasets/surface-pm2-5-archive/) | `airQuality.tif`                            | Ground-level fine particulate matter (PM2.5)                             | μg/m<sup>3 | ~1 km    | Single mean of years 2014-2019
Global Heat Stress       | Naia <br> [CHIRTS](https://data.chc.ucsb.edu/products/CHIRTSdaily/v1.0/global_tifs_p05/)               | `heatstress103.tif`                         | Number of days in year where heat index was greater than 103F            | #          | ~5 km    | Year 2016 (hottest up to 2020)         
Global Sanitation Access | Naia <br> [IHME](https://ghdx.healthdata.org/record/ihme-data/lmic-wash-access-geospatial-estimates-2000-2017)                 | `sanitation_access.tif`<sup>[1](#fn1)</sup> | Coverage of any improved sanitation facility access in 90 LMICs          | %          | ~5 km    | Single mean of years 2012-2017<sup>[3](#fn3)</sup>         
Global Stunting          | Naia <br> [IHME](https://ghdx.healthdata.org/record/ihme-data/lmic-child-growth-failure-geospatial-estimates-2000-2017)                 | `stunting.tif`<sup>[2](#fn2)</sup>          | Prevalence of stunting for children under 5 years of age for 105 LMICs   | %          | ~5 km    | Single mean of years 2012-2017<sup>[3](#fn3)</sup>          
Countries                | [World Bank](https://datacatalog.worldbank.org/search/dataset/0038272/World-Bank-Official-Boundaries)                     | `WB_countries_Admin0_10m`                   | World Bank-approved administrative boundaries (Admin 0), i.e. countries  | NA         | NA       | Last updated Mar 19, 2020        

<a name="fn1">1</a>: Previously named `w_access`

<a name="fn2">2</a>: Previously named `IHME_LMIC_CGF_2000_2017_STUNTING_PREV_MEAN_2017_Y2020M01D08.tif`

<a name="fn3">3</a>: Confirm with Naia that it was not the last 3 available years of data, or 2014-2017

## Software

This chatbot relies on the LangChain interface and the Llama 3.1 large language model (LLM). All necessary packages are 
included in `environment.yml`, but you will have to [download Ollama](https://python.langchain.com/docs/integrations/chat/ollama/) 
(once) to your local computer before you are able to access the Llama 3.1 model. Keep the Ollama application running to 
use the model within your python environment.

## Scripts
- `tif_to_sql.py`: Functions to calculate zonal statistics for every polygon over a given raster and load dataset into local SQLite database
- `model.py`: Builds LangChain model from pre-trained Llama 3.1 LLM with chained agents that computes SQL query on SQLite database and return a natural language response
- `chatbot.py`: Master script for "user" interaction that runs model and returns answer to a given user question
- `sandbox.py`: Script for development of new functionality, that may or not be working yet, but is not yet integrated into full workflow

## To-do 
- [ ] Relational SQL database for all data layers
- [ ] Metadata for data layers that can be queried and/or used to give context to responses
- [ ] Retain chat history so that bot is more conversational
- [ ] Further prompt engineering (e.g., ignore nan values) and few-shot classification
- [ ] Load and query population data as estimate of exposure
- [ ] Set up unit tests for performance of bot during development
- [ ] Present response geospatially in addition to text
- [ ] Time series data and queries

