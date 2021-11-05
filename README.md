# My_Learning_Tracker

Log of professional development and data science skills, learning, resources, article/book notes highlights, and anything else noteworthy. Reverse chronological order.


### 2021/11/05

Purchase of new Macbook Air M1, wrote google drive document on setup of new machine for data science use: https://docs.google.com/document/d/12b7zky7er_mWstumzcEIfo-DiNTpeyDT2edFJJHvouQ/edit

Setup git folder structure for exercises for https://www.amazon.ca/gp/product/1492032646/ref=ppx_yo_dt_b_asin_title_o00_s00?ie=UTF8&psc=1.

### 2021/04/04

Completed work project transition, for employer M, starting in July 2020 and completing with transition of project to sustaining resources. I functioned as main model lead, designer of DOE/sampling, process SME, and primary contact with operations.

### Project Overview

Supervised learning with a target of NaCl content in finished product. Process of interest is a multi-stage industrial crystallization process, in which 4 XLR (crystallizer) vessels each product a slurry output with a target NaCl value. 

**Process**

An industrial processing operation consisting of 4 large XLRs, interlinked and operating in sequence. Each XLR takes an input stream, produces a slurry (containing the target variable) with the bulk of hte process fluid continuing for further rework in subsequent XLRs. The slurry ouputs of XLRs 1, 2 and 3 are input into the both of XLR4 along with the mother liquor for final processing. There are significant processes operating both upstream, alongside, and downstream of the vessels.

**Data**

Two primary components: 
  1. Multiple (200+) continously monitored process 'tags' such as flow, chemistry, or other measurements of physical or chemical processes. Some shared between vessels, others unique.
  2. Labelled target data which is sampled with analytical evaluation of the NaCl content. This analysis is done asynchronously, samples are collected only 1/week.

**Business case**

Implementation of predictive models would allow for tighter operational loop for improved operations. Currently the labelled target attribute is only availalbe 1/week for each vessel. The operational state is self-correlated only within 3hrs.

Additionally, a lot of the value of the project was in the development of the technology stack within M, and the process of building and operationalizing the first ML models. This was the first of any project of this type within the org, and predates the development of a corporate datalake.

### Project Architecture/Stack

Data: SQL Wonderware source (Azure Datalake available but not enabled)
ETL: RapidMiner desktop, embedded Python scripts
Modelling: RapidMiner desktop
Data prep, offline analysis: R, Python
Documentation: Azure Devops Wiki, Microsoft Teams
Source control: Azure Devops w/ Gitextensions desktop client
Deployment: RapidMiner AI hub, WebAPI for automated retraining/redeployment
Model output: Operations interface webservice via intraweb
PowerBI: Training ouput for local Datascientist use

### Implementation

**Supplemental work**

Two supplmental sampling campaigns were completed:
  1. Samples taken on a 1hr strict schedule for 96 hrs for a single XLR vessel. This enables ARIMA analysis to be complete (data was stationary), with a self-correlation of 3hrs to be found. This allows for aggregation of the continous tags to be done to a 3hr interval to reduce the volume of the training set.
  2. Samples taken on a 3hr loose schedule during a 20 day long Design Of Experiment (DOE) on all 4 XLRs. This leads to an increased density of training data, as well as additional data of conditions beyond normal operational setpoints.

**Data prep**

Continous and analytical data was queried using SQL into the RapidMiner desktop ap, queries seperated by month and lagged 1.5 minutes between queries to avoid server issues. Aggregate of files into local Bronze datasets.

Analysis of timeseries data done for missing data, cross correlation between variables using Python/R scripts. Exclusion lists generated from these and embedded into Rapidminer for ETL pipeline.

Rule-based filters were generated to note individual rows as normal, outlier, outages, or insufficient data. This allows for removal of upset conditions from the training set.

One-hot encoding of a few attributes were completed: Vessel status (most value to provide evaluation for project data scientists), and time-of-day encoding for analytical samplign set, as well as evaluation of continous data cylicality.

Significant effort went into evaluation (manually and automatically) of appropriate training sets for each vessel to reflect operational, controls, and capital project changes which have occured in time. It was found to be exceptionally important to model accuracy that the training set reflects the current operational paradigm. Training and test split were done in blocks of time which were explicity set.

**Model control/data engineering**

Python script used to generate a dataframe of various configurations to generate for use in the Rapidminer modelling processes. This enabled things such as model type, sycronization of data, data prep Regex config, training/test split, preprocessing, postprocessing, generation of lagged/lead, moving averages, and other features to be turned off and on. Total of ~20 model features. The build of each configuration was explicit in name convention. As well as user specified configurations, randomly seeded permutations of all options can be generated to allow for genetic selection of useful features.

Multiple model types run for each configuration with data prep output, model results, and scoring savings into a Silver datastructure, as well as output via csv for PowerBi modelops analysis.

Once a model and configuration is selected for deployment, a Gold datastructure is generated. This can be deployment to either Dev or Prod environments of the Rapidminer server, which runs on a schedule and populates the operational interface.

**Models**

Different models were found to function best based on operational scenarios. Several model types were trained and evaluated, with types Gradient Boosted Trees working in temporality earlier processes, and Generalized Linear Models working the temporaly later processes. A stacked model type of these worked well in intermediate vessels. Evaluation of other model types were completed (including Deep learning, Random Forest, SVM, and others). Deep learning appeared to have promise, however the sparsity of the training set hampered performance. Regardless, the accuracy of the deployed 4 models was found to be quite high.

For each vessel, additional models were generated which target the future value of the NaCl value a single instance into the future (3hrs). This was done via building of a submodel stack in which each input tag had a model associated to it. Overall the value of this approach was found to be not high, and this aspect of the project was abandoned. Accuracy could not be achieved, and there was little operational value. The move from 1/wk avaialability of the target variable to every 3 hrs realized the bulk of the value.

Various model pre-processing strategies were implemented (Kmeans, ICA, PCA) in attempts to improve model accuracy. Some of these were found to function reasonably well (lower order PCA), however limitations on grouped model types with the current RapidMiner server package blocked deployment past desktop.

During model development, an emphasis was placed on early testing with operations, which was helpful in determining additional data prep and transformation steps required. Pre-operational interface, models were trained overnight on the most recent data, and reviewed via screen share of a local PowerBI report, with a transition to review of the intra-net model output site roughly halfway through the project.

**Sustainability**

In order to ensure operational sustainability of the deployed projects, two methods of model evaluation were designed. One is color coding of the output data in the operations interface based on statistical methods of the training data set. This provides information on the value of the training set for a given prediction. The second is the use of autoencoders using H2O package in Python to determine if any change is present in the last 30 days of training data vs historical training set. This is operationalized via a Python script in RapidMiner.

Tracking of use and feedback from operations is collected voluntarily each day when models are reviewed. Compliance with use and provision of feedback continues to be high.

**Change Management**

Significant efforts were complete to manage change at the site and corporate level, as this was the first attempt at a machine learning install within M. Overall this worked well, with operations being involved early and throughout the process. Additional efforts were made to be open about failures and learnings to the site and others.

### Project Outcomes

Models were succesfully generated for all 4 XLR vessels, with the data ETL, training, and deployment processes all moved to server operations. Site stakeholder operations are using the outputs daily, and continue to provide feedback. Site management is satisfied with the project, and M has now built an integrated ML team which will lead these types of projects in the org. Currenly, additional resources are being onboarded, and evaluation of the next candidate projects is underway.



### 2020/04/18

Worked on creating an animated scatter plot of child mortality rates vs GDP using matplotlib.animation. Sourced data, cleaned, prepped, and can plot a single instance. Ran out of time to learn animated, will review docs tomorrow. Seems fairly straightforward, but may need to modify structure of dataframes.

### 2020/04/17

Further development of covid choropleth workbook.

Added dynamic collection of files directly from source Github, save local copy of dataframe and chorpleths with datestamp. Change choropleth to update scale automatically based on current upper bound of cases/deaths.

Easy updates to functionality, and now can track progress over time. May have issues with scales on choropleths if wanted to build a live version. Would need to consider using potential upper bounds to start with if planning a live version.

### 2020/04/15-16

Multiple day project committed to Github.

 __Covid_19_Choropleth_Canada__

_Goal_

Generate choropleths of the spread of Covid-19 in Canada as of 2020/04/16 for educational purposes. There is no intention of distribution.

This repository included the working jupyter notebook as well as .csv files: mortality data, infection data, relational data for health regions, source .shp files and associated records.

_Results_

Data on infections and deaths per health region was sourced (in a precompiled state) from Github. These two sources were combined, along with census information on population per health region to calculate mortality and infection rates, and an index of health region names and ids.

Goverment of Canada .shp file was sourced for creation of Choropleths.

Data was cleaned/munged and cleanup of text was required to align source data for matching with .shp region ids.

Current maximum infection rate was found to be 3.4/1000 persons, with a death rate of .18/1000 persons. Both infections and deaths were found to be concentrated in metropolitan areas of Vancouver, Toronto, as well as widely throughout the province of Quebec. Minimal infections and deaths are found in the more sparsely populated areas of central and western Canada.

_Issues_

Reporting of cases for the 3 health regions of the Fraser Valley area, as well as the 3 Vancouver Island regions are not seperated into distinct health regions but reported as a whole. Without inspecting individual cases it is difficult to report these areas accurately. These areas represent a nominal volume of cases compared to the whole of Canada.

_Acknowledgements_

Data was sourced from https://github.com/ishaberry/Covid19Canada (accessed 2020/04/15)
  
__Notes from project:__

Working with multiple data sources was challenging and time intensive. A lot of effort in munging and cleaning could have been saved if all source files were collected and viewed prior to determine an efficient way of establishing a common index. Working with text with slightly different formatting and naming conventions was difficult as well.

In future, would be worthwhile to generate custom .csv files for conversions and relational structures that could be reused. This is domain specific.

Also found limitations with the .shp file projections as the extents of the northern regions continue to the north pole. They are not extremely visually appealing and thus would need further processing prior to wider distribution.

Overall a difficult project but worthwhile.

### 2020/04/14

Completed miniproject/tutorial on sns heatmaps for correlation covariance matrices. 

https://towardsdatascience.com/annotated-heatmaps-in-5-simple-steps-cc2a0660a27d. Generating boolean mask for upper triangle, as well as dummy encoding for categorical variables. Seems quite eaasy and quick, as well as a good useful tool.

### 2020/04/12

__Project: Webscraping IMBD/Metacritic__

Completed a webscraping tutorial and used the methodology to use html parsing with BeautifulSoup4 to crawl a website for analysis. Project goal is to scrape 2000 ratings from each of IMBD and metacritic. 

It is worthwhile to examine the source websites, since this may mean as much as 4000 requests (~1 sec/request = ~hr total). A URL for advanced search was found at IMDB.com which had 50 titles with all desired information. This reduced the number of requests to 80, with custom URL requests to crawl site at random delay intervals between 1-15secs.

_Scraping methodology used:_
1. Examine source for most efficient way to get max data on a single page
2. Examine URL to see if iterative scraping will make sense at that page
3. Scrape a single record and verify
4. Scrape a single page and verify, put records into lists
5. Scrape all pages using variable to iterate though URL, put records into lists
6. Convert lists to dataframe
7. Complete formatting of dataframe
8. Complete analysis of dataframe

_Munging:_
Year information was cleaned and converted to int. IMBD ratings were normalized for comparison.

_Outcome:_
IMDB ratings tend to have a heavy central tendency around 71%, with a std of 7.3

Metacritic ratings tend to have a more widely distributed score with a mean of 63%, and a std of 15.6. Metacritic ratings also shows relatively few ratings in the areas of 50% and 75%. Perhaps this is due to a tendency to avoid round quartile scores.

Project was uploaded into Github as a repo, a readme file was written.

__Reading__

https://qz.com/968101/how-elon-musk-learns-faster-and-better-than-everyone-else/

_Notes:_
* Success of expert/generalists over time. These individuals exist.
* Read. A lot. Broadly. Take notes.
* Don't waste time, work very hard. Get off social media. Never read the same thing twice (special note to self -reddit is particularly bad here, scrolling endlessly and reading comments is a huge waste of time)
* Learning transfer is critical. Take Kernel and apply to real world.
* Step 1: deconstruct knowledge into principles. Semantic tree building.
* Step 2: reconstruct principles into a new field. 
*   AI, tech, physics and engineering into: aerospace, auto, trains, aviation, paypal, openAI.
* What does this remind me of, why does it remind me of it?


### 2020/04/11

Read a few chapters in textbook "Interpretable machine learning", interesting points on how to score a given algorithm for interpretability (direct score ie: # of layers/tree depth vs looking at input vs outputs), as well as instances where you would want (high risk, new appliactions, impactful results) or not care about interpretability (basic regression, well understood, small scope problems).

Completed tutorials on BeautifulSoup to scrape weather data from US and Canadian weather services, interpret the html, and import into a DataFrame for further analysis.


### 2020/04/10

Continued work on image recognition of clothing. Implemented linear and quadratic SVM for comparison vs Sklearn libraries (with evaluate of c at 0.01/0.2/1/10/100). Neural net has the feature of seeing weights of probabilties of each outcome, this could be useful in real world applications (such as image recognition for self driving applications). Noted that quadratic SVM has an extremely long execute time.

Uploaded to Github, wrote readme.

### 2020/04/08

Image recognition of clothing using Tensorflow/Keras. Committed to github.

Download and use mnist_fashion. 60000 training/10000 test of clothing/footwear in 10 categories, 28x28 pixel image.

Built prediction neural net to predict with accuracy 91% train, 88% test. ReLU activation, 3 layers sequential, optimizer 'Adam', losses crossentropy, metric accuracy.

Reviewed userDoc for Keras to undestanding terminology, model & compiler. Overall seems fairly easy/intuative to use, documentation is good, although more research will be needed to understand loss functions, activations and metrics available through Keras.

### 2020/04/07

Attempted to set up a google analytics query API, but was unable to successfully get the OAuth2 script to work on the local machine. Will come back to this later. Need to understand background on API better, so read though:

https://docs.google.com/presentation/d/19bP_khVtanOgCmoEg8qKoLjhh01rz1TFuss12MFz96E/edit#slide=id.p

**Notes:**

API analagous to http between user/server. GET vs POST.

HTTP status codes:

|Code | Meaning            |
|-----|:-------------------|
|2xx  | Success            |
|3xx  | Redirect (success) |
|4xx  | Client errors      |
|400  | Bad request        |
|403  | Forbidden          |
|404  | Not found          |
|405  | Not allowed        |
|5xx  | Server error       |
|6xx  | Custom error       |

e**X**ensible **M**arkup **L**anguage vs **J**ava**S**script **O**bject **N**otationformatting

**A**pplication **P**rogramming **I**nterface

Authorization for GET/POST done via: inclusion in request, OAuth (3rd party service - the worst), ID/PW

Anatomy of a GET request:

https:// Root URL/API url/API endpoint(action)/?(indicated following is params)/Param1&Param2

*Tools:* 

* cURL, Postman, browser via extension

* Look for local libraries (python etc) for any substaintial service. This will make life easier to interact with the API.

Also did some work with JSON files, converting with load(s) and dump(s) to learn to convert JSON files into str.

### 2020/04/06

Build a local settings.py file to hold private API and google CSE keys, modified prior workbook to use this file.

### 2020/04/05

Mini-project completed to learn to scrape web for results on topic/geo/recency.

Tutorial followed: https://towardsdatascience.com/current-google-search-packages-using-python-3-7-a-simple-tutorial-3606e459e0d4

Two methods learned:

* Googlesearch - fast, easy, no API required. Limited to pulling URLs only, only from google mainpage search results, and also a mandatory 2 second delay between queries.

* Googlequery - Access custom search via google API. More involved, requires setting up an API. Can be used to pull from any website via customsearch. Also able to get metadata, as well as some html.

### 2020/04/04

Completion of personal project on canadian CPI vs wages vs individual costs complete. Committed to Github.

*Technical skills:*

Learned additional functionality of Seaborn including FacetGrid, and use of dataframe feature to allow for use of 'Hue' within a plot. Installed and learned to use Tabulate for in-notebook reporting. Experienced difficulty with labelling of lines on graphs directly coupled with very busy/non intuative line graphs. Take-away is to limit features to 5-7 items, or use color coding to show 3rd dimensions.

*Project findings:*

Overall, increase in cost of essential and non essentials has been lower than that of median average wages. The official CPI has seen an incerase in 145% while wages growed 171% over the same period.

Some specific categories have outstripped wage growth: Education and education related expenses, utility bills, gasoline (one $/L basis, does not account for higher incremental fuel efficiency), and tobacco products.

It is not clear from the data how the cost of housing is calculated. In the case of rent, there is a clear direct cost, however the cost of purchased housing is unclear as to the impact of purchase price vs mortgage vs carrying costs. As well, there is no differention across specific geographies in the data available.

Overall the conclusion can be that other than for some specific categories, consumer prices for essential and non-essential goods have increased at a slower rate than median weekly wages, implying a higher effective purchasing power for consumers from 1997 to 2019.
