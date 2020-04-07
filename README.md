# My_Learning_Tracker

Log of professional development and data science skills, learning, resources, highlights, and anything else noteworthy. 

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

Notes:
API analagous to http between user/server. GET vs POST.
HTTP status codes:

2xx - success
3xx - redirect (success)
4xx - client errors
400 - Bad request
403 - Forbidden
404 - Not found
405 - Not allowed - GET a POSTable thing
5xx - server error
6xx - custom error

eXensible Markup Language vs JavaSscript Object Notationformatting
Application Programming Interface

Authorization for GET/POST done via: inclusion in request, OAuth (3rd party service - the worst), ID/PW

Anatomy of a GET request:

https:// 
<div class = 'text-blue mb-2'>
              Root URL
              </div>
              /API url/API endpoint(action)/?(indicated following is params)/Param1&Param2

Tools: 

cURL, Postman, browser via extension

Look for local libraries (python etc) for any substaintial service. This will make life easier to interact with the API.


