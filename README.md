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

Example Call

`https://<ec_instance_ip>/api/search/v1/data.json?category=Tools&facet=sentiment&starRating=2&api-key={my_token}`


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
Use filters to narrow the scope of your search. You can specify the fields and the values that your query will be filtered on.

Filter Query Parameters

* starRating
* helpfulVotes
* date
* startDate
* endDate
* category
* reviewID

## USING FACETS TO OBTAIN AGGREGATES
Use facets to view the aggregates of the search terms.

The following fields can be used as facet fields: 
* avg 
* sentiment
* max
* min

Specify facets using the facet parameter. Set facet=<field> and the response will contain an array with a count for the top 3 terms that have the highest count for each facet.

Query with aggregates average and sentiment:

`https://<ec_instance_ip>/api/search/v1/data.json?category={cat}&facet=avg&facet=sentiment&api-key={my_token}`

By default facet counts ignore all filters and return the count for all results of a query. 

Here is the facet array response to the query.

{'aggregate': {'average': star_rating          2.000000
helpful_votes        0.166667
total_votes          0.500000
vine                 0.000000
verified_purchase    0.833333
'max': star_rating          2.0
helpful_votes        1.0
total_votes          1.0
vine                 0.0
verified_purchase    1.0
, 'min': star_rating          2.0
helpful_votes        0.0
total_votes          0.0
vine                 0.0
verified_purchase    0.0
, 'sentimentScore': -0.06798941798941802}
      ...
**Examples Requests**
Search for documents containing new york times and return results 20-29 with results sorted oldest first.
      
`https://<ec_instance_ip>/api/search/v1/data.json?category={cat}&facet=avg&facet=max&facet=min&facet=sentiment&starRating={star}&api-key={my_token}`

**Example Response**
Here is an partial example response.

{
   "aggregate":{
      "average":"star_rating          2.000000
helpful_votes        0.166667
total_votes          0.500000
vine                 0.000000
verified_purchase    0.833333
Name":"mean",
      "dtype":float64,
      "max":"star_rating          2.0
helpful_votes        1.0
total_votes          1.0
vine                 0.0
verified_purchase    1.0
Name":"max",
      "dtype":float64,
      "min":"star_rating          2.0
helpful_votes        0.0
total_votes          0.0
vine                 0.0
verified_purchase    0.0
Name":"min",
      "dtype":float64,
      "sentimentScore":-0.06798941798941802
   },
   "data":[
      {
         "marketplace":"US",
         "customer_id":"23260912",
         "review_id":"R2XYDBMHUVJCSX",
         "product_id":"B00PFZFD8Y",
         "product_parent":"344168617",
         "product_title":"NatraCure Plantar Fasciitis Wrap (One Wrap) - 1291-S CAT Arch Support (Small/Medium)",
         "product_category":"Health & Personal Care",
         "star_rating":2,
         "helpful_votes":0,
         "total_votes":1,
         "vine":0,
         "verified_purchase":1,
         "review_headline":"Two Stars",
         "review_body":"I wear it a few times only but the fabric cover the jelly pad already broken.",
         "review_date":"Timestamp(""2015-08-31 00:00:00"")"
      },
              ...
Limit Fields in Response
You can limit the number fields returned in the response with the limit parameter.

Resource Types
URIs are relative to https://<EC2_Instance_API_Address>/huggingface/search/v1 unless otherwise noted.

Review Data
For more information see Review Data.
Method	Endpoint	Description
GET	/reviewdata.json	
Search Hugging Face US Amazon Review Data Set by keywords and filters.


## TODO

* Add error handling for date and start/end date. Can only add either or. 
* Add error handling for when users add an end date that is sooner than start date
* Add pagination to api functionality
* Add proper authorization
* Encrypt tokens and/or add as environment variable (.env)
* Add tokenization to sentiment score to make score more accurate


