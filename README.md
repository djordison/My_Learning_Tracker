# My_Learning_Tracker

Log of professional development and data science skills, learning, resources, article/book notes highlights, and anything else noteworthy. 

### 2020/04/04

Completion of personal project on canadian CPI vs wages vs individual costs complete. Committed to Github.

*Technical skills:*

Learned additional functionality of Seaborn including FacetGrid, and use of dataframe feature to allow for use of 'Hue' within a plot. Installed and learned to use Tabulate for in-notebook reporting. Experienced difficulty with labelling of lines on graphs directly coupled with very busy/non intuative line graphs. Take-away is to limit features to 5-7 items, or use color coding to show 3rd dimensions.

*Project findings:*

Overall, increase in cost of essential and non essentials has been lower than that of median average wages. The official CPI has seen an incerase in 145% while wages growed 171% over the same period.

Some specific categories have outstripped wage growth: Education and education related expenses, utility bills, gasoline (one $/L basis, does not account for higher incremental fuel efficiency), and tobacco products.

It is not clear from the data how the cost of housing is calculated. In the case of rent, there is a clear direct cost, however the cost of purchased housing is unclear as to the impact of purchase price vs mortgage vs carrying costs. As well, there is no differention across specific geographies in the data available.

Overall the conclusion can be that other than for some specific categories, consumer prices for essential and non-essential goods have increased at a slower rate than median weekly wages, implying a higher effective purchasing power for consumers from 1997 to 2019.

### 2020/04/05

Mini-project completed to learn to scrape web for results on topic/geo/recency.

Tutorial followed: https://towardsdatascience.com/current-google-search-packages-using-python-3-7-a-simple-tutorial-3606e459e0d4

Two methods learned:

* Googlesearch - fast, easy, no API required. Limited to pulling URLs only, only from google mainpage search results, and also a mandatory 2 second delay between queries.

* Googlequery - Access custom search via google API. More involved, requires setting up an API. Can be used to pull from any website via customsearch. Also able to get metadata, as well as some html.

### 2020/04/06

Build a local settings.py file to hold private API and google CSE keys, modified prior workbook to use this file.

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

### 2020/04/08

Image recognition of clothing using Tensorflow/Keras. Committed to github.

Download and use mnist_fashion. 60000 training/10000 test of clothing/footwear in 10 categories, 28x28 pixel image.

Built prediction neural net to predict with accuracy 91% train, 88% test. ReLU activation, 3 layers sequential, optimizer 'Adam', losses crossentropy, metric accuracy.

Reviewed userDoc for Keras to undestanding terminology, model & compiler. Overall seems fairly easy/intuative to use, documentation is good, although more research will be needed to understand loss functions, activations and metrics available through Keras.

### 2020/04/10

Continued work on image recognition of clothing. Implemented linear and quadratic SVM for comparison vs Sklearn libraries (with evaluate of c at 0.01/0.2/1/10/100). Neural net has the feature of seeing weights of probabilties of each outcome, this could be useful in real world applications (such as image recognition for self driving applications). Noted that quadratic SVM has an extremely long execute time.

Uploaded to Github, wrote readme.

### 2020/04/11

Read a few chapters in textbook "Interpretable machine learning", interesting points on how to score a given algorithm for interpretability (direct score ie: # of layers/tree depth vs looking at input vs outputs), as well as instances where you would want (high risk, new appliactions, impactful results) or not care about interpretability (basic regression, well understood, small scope problems).

Completed tutorials on BeautifulSoup to scrape weather data from US and Canadian weather services, interpret the html, and import into a DataFrame for further analysis.

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

### 2020/04/14

Completed miniproject/tutorial on sns heatmaps for correlation covariance matrices. 

https://towardsdatascience.com/annotated-heatmaps-in-5-simple-steps-cc2a0660a27d. Generating boolean mask for upper triangle, as well as dummy encoding for categorical variables. Seems quite eaasy and quick, as well as a good useful tool.

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

### 2020/04/17

Further development of covid choropleth workbook.

Added dynamic collection of files directly from source Github, save local copy of dataframe and chorpleths with datestamp. Change choropleth to update scale automatically based on current upper bound of cases/deaths.

Easy updates to functionality, and now can track progress over time. May have issues with scales on choropleths if wanted to build a live version. Would need to consider using potential upper bounds to start with if planning a live version.

