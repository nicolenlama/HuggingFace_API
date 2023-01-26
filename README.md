# HuggingFace_API
API to access public Hugging Face Amazon Review Dataset using Django framework


# Review Search
## Overview
PATHS
/reviewData.json
GET
COMPONENTS

Use the Hugging Face Amazon US API to look up reviews by keyword. You can refine your search using filters and facets.
Must specify category in the URL

/reviewData.json?category={category}&q={query}&fq={filter}
Example Call
https://<ec_instance_ip>/api/search/v1/reviewData.json?q=excellent&api-key=yourkey

**Categories**

* Wireless
* Watches
* Video_Games
* Video_DVD
* Video
* Toys
* Tools
* Sports
* Software
* Shoes
* Pet_Products
* Personal_Care_Appliances
* PC
* Outdoors
* Office_Products
* Musical_Instruments
* Music
* Mobile_Electronics
* Mobile_Apps
* Major_Appliances
* Luggage
* Lawn_and_Garden
* Kitchen
* Jewelry
* Home_Improvement
* Home_Entertainment
* Home
* Health_Personal_Care
* Grocery
* Gift_Card
* Furniture
* Electronics
* Digital_Video_Games
* Digital_Video_Download
* Digital_Software
* Digital_Music_Purchase
* Digital_Ebook_Purchase
* Camera
* Books
* Beauty
* Baby
* Automotive
* Apparel
* Digital_Ebook_Purchase
* Books


## FILTERING YOUR SEARCH
Use filters to narrow the scope of your search. You can specify the fields and the values that your query will be filtered on. The Article Search API uses Elasticsearch so the filter query (fq) uses standard Lucene syntax. Separate the filter field name and value with a colon and surround multiple values with parentheses.

field-name:("value1" "value2" ... "value n")
The default connector for values in parentheses is OR. If you declare an explicit boolean value it must be capitalized. You can filter on multiple values and fields.

field-name-1:("value1") AND field-name-2:("value2" "value3")
For a list of all fields you can filter on see the Filter Query Fields table below.

You can also filter by search text.

**Pagination**

The Article Search API returns a max of 10 results at a time. The meta node in the response contains the total number of matches ("hits") and the current offset. Use the page query parameter to paginate thru results (page=0 for results 1-10 page=1 for 11-20 ...). You can paginate thru up to 100 pages (1000 results). If you get too many results try filtering by date range.

Filter Query Examples
Restrict your search to articles with The New York Times as the source:

fq=source:("The New York Times")
Restrict your search to articles from either the Sports or Foreign desk:

fq=news_desk:("Sports" "Foreign")
Restrict your search to articles that are about New York City and from the Sports desk:

fq=news_desk:("Sports") AND glocations:("NEW YORK CITY")
If you do not specify a field the scope of the filter query will look for matches in the body headline and byline. The example below will restrict your search to articles with The New York Times in the body headline or byline:

fq=The New York Times
Find reviews with the word "excellent" that were on the print papers front page.

fq=excellent AND print_page:1 AND (print_section:("A" "1") OR (!_exists_:print_section))
Filter Query Fields

* starRating
* helpfulVotes
* date
* startDate
* endDate
* category
* reviewID

USING FACETS
Use facets to view the relative importance of certain fields to a search term and gain insight about how to best refine your queries and filter your search results.

The following fields can be used as facet fields: starRating sentimentScore helpfulVotes.

Specify facets using the facet_fields parameter. Set facet=true and the response will contain an array with a count for the top 3 terms that have the highest count for each facet. For example including the following in your request will add a facet array with a count for the top five days of the week to the response.

facet_fields=day_of_week&facet=true
By default facet counts ignore all filters and return the count for all results of a query. 

q=obama&facet_fields=source&facet=true&begin_date=20120101&end_date=20121231
You can force facet counts to respect filters by setting facet_filter=true. Facet counts will be restricted based on any filters you have specified (this includes both explicit filter queries set using the fq parameter and implicit filters like begin_date).

Here is the facet array response to the query.

"facets": {
  "source": {
    "_type": "terms"
    "missing": 524
    "total": 83121
    "other": 317
    "terms": [
      {
        "term": "The New York Times"
        "count": 68530
      }
      {
        "term": "AP"
        "count": 7705
      }
      {
        "term": "Reuters"
        "count": 4969
      }
      {
        "term": "International Herald Tribune"
        "count": 1464
      }
      {
        "term": ""
        "count": 136
      }
    ]
  }
}
If you set facet_filter to true the facet array will only count facet occurences in 2012.

facets": {
  "source": {
    "_type": "terms"
    "missing": 192
    "total": 22596
    "other": 139
    "terms": [
      {
        "term": "The New York Times"
        "count": 14812
      }
      ...
Examples Requests
Search for documents containing new york times and return results 20-29 with results sorted oldest first.

https://api.nytimes.com/svc/search/v2/articlesearch.json?q=new+york+times&page=2&sort=oldest&api-key=your-api-key
Search for all documents published on January 1 2012 containing romney. Facet count will be returned for day_of_week and will be reflective of all documents (i.e. the date range filter and filter query do not affect facet counts).

https://api.nytimes.com/svc/search/v2/articlesearch.json?fq=romney&facet_field=day_of_week&facet=true&begin_date=20120101&end_date=20120101&api-key=your-api-key
Example Response
Here is an partial example response.

{
  "response": {
    "meta": {
      "hits": 25
      "time": 332
      "offset": 0
    }
    "docs": [
      {
        "web_url": "http://thecaucus.blogs.nytimes.com/2012/01/01/virginia-attorney-general-backs-off-ballot-proposal/"
        "snippet": "Virginias attorney general on Sunday backed off of a proposal to loosen the states ballot access rules to allow more Republican presidential candidates to qualify."
        "lead_paragraph": "DES MOINES -- Virginias attorney general on Sunday backed off of a proposal to loosen the states ballot access rules to allow more Republican presidential candidates to qualify."
        ...
      }
    ]
    "facets": {
        "day_of_week": {
            "_type": "terms"
            "missing": 1871790
            "total": 13098462
            "other": 3005891
            "terms": [
              {
                "term": "Sunday"
                "count": 3122347
              }
              ...
Limit Fields in Response
You can limit the number fields returned in the response with the fl parameter.

fl=web_url
Resource Types
URIs are relative to https://<EC2_Instance_API_Address>/huggingface/search/v1 unless otherwise noted.

Review Data
For more information see Review Data.
Method	Endpoint	Description
GET	/hfreviewdata.json	
Search Hugging Face US Amazon Review Data Set by keywords and filters.


