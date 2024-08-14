# Module 6 Challenge (data-sourcing-challenge)

The module 6 challenge allows for practice in sourcing and retrieving data via available APIs.  In this exercise the json data is retrieved and processed into a DataFrame where it can be evaluated, merged and cleaned as appropriate. 

In summary we are retrieving data for movie reviews from the New York Times article search.  That data is used to attempt to load movie specific data from the Movie Database.  This data is merged and saved as a .csv file. 

## Use of the NYT API

Our first step is to find movie review data from the New York Times (https://developer.nytimes.com/docs/articlesearch-product/1/overview). 

We are searching for movie reviews within a specific date range that involve 'love'.  The API will return 10 articles as json data per page and requires that we rest between page calls.  This rest time is currently set to 12 seconds.  

In the event of an exception retrieving the HTTP request and json data a notification will be presented and further data will stop being processed.  

The execution will proceed until the configured number of pages is reached (currently 20) or no data is returned.

Data is stored as a DataFrame utilizing .json_normalize as there are nested lists in the data returned.  

The keywords for the data are cleaned and a title column is added based on extraction of data from the headline.main column.  This is extracted as a list to be used to look up movie data via the TMDB API in our next step.

## Use of TMDB API

NYT review data is used to look up movie data via the API from TMDB (https://developer.themoviedb.org/docs/search-and-query-for-details)

We first utilize the movie search API to identify the unique key for the movie id.  If found, this is used to retrieve data for the movie.  Not all movies will be found and a message will be out for the status of all movie lookups.

Identified movies will be stored in a list of dictionary items representing the movie data and processed as DataFrame.  

## Merging and cleaning Data
Data from the NYT and TMDB DataFrames will be merged based on the `title` column.  There should not be overlap in columns between the data outside of `title` used to merge. 

We will clean the data as follows:  

1. Data in columns for `genre`, `spoken_languget`, and '`production_countries` will be cleaned as string values
2. The `byline.person` column is dropped
3. Duplicates are removed
4. The index is reset

All data is stored to the filesystem as `collected_data.csv`

## HTTP Exception processing
The following exceptions are tracked for all API calls in this exercise, including the NYT API and the TMDB API calls.

1. requests.exceptions.HTTPError 
2. requests.exceptions.ReadTimeout
3. requests.exceptions.ConnectionError
4. requests.exceptions.RequestException