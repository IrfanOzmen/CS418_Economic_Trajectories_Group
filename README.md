# CS418_Economic_Trajectories_Group
Group members: Irfan, Briana, Masa, Harsh
### Research Question: What characteristics of online reviews best predict future changes in a restaurant or local business's sales, popularity, or likelihood of closure?

## Primary datasets:

### 1. Yelp Open Dataset
[Yelp Open Dataset](https://business.yelp.com/data/resources/open-dataset/?utm_source)
#### Yelp Business Data

**Shape**: (150346, 14)

Each row represents one business

**Columns we care about and their types:**
* business_id - object
* name - object
* address  - object
* city - object
* state - object
* postal_code - object
* latitude - float64
* longitude - float64
* stars - float64
* review_count - int64
* is_open - int64
* attributes - object
* categories - object
* hours - object
* dtype: object


#### Yelp Review Data
**Shape**: (100000, 9)

**Each row is one review written for one business**

**Time**: March 1, 2005 - October 4, 2018

**Columns we care about and their types:**
* review_id              object
* user_id                object
* business_id            object
* stars                   int64
* useful                  int64
* funny                   int64
* cool                    int64
* text                   object
* date           datetime64[ns]
* dtype: object


### 2. Google Local Reviews Dataset
[Google Local Reviews Dataset - UCSD](https://cseweb.ucsd.edu/~jmcauley/datasets.html)

#### Google Local Business Metadata

**Shape**: (179205, 15)

Each row represents one business in Illinois

**Columns and types:**
* name                 object
* address              object
* gmap_id              object
* description          object
* latitude            float64
* longitude           float64
* category             object
* avg_rating          float64
* num_of_reviews        int64
* price                object
* hours                object
* MISC                 object
* state                object

**Restaurant subset**: (28809, 15)

The restaurant subset contains businesses with a category containing the word restaurant

#### Google Local Reviews
**Shape:** (100000, 8)

Each row represents one review of a business

**Important columns and types:**
* rating - int64
* text - object
* pics - object
* resp - object
* gmap_id - object

**Time:** January 29, 2008 - September 8, 2021

The Google Local dataset provides business information and customer reviews for Illinois businesses. The restaurant subset is used for this project, and the two files are linked using gmap_id

### Secondary datasets:

### 3. Chicago Food Inspections 
[Chicago Food Inspections](https://data.cityofchicago.org/Health-Human-Services/Food-Inspections/4ijn-s7e5/about_data)

Chicago Business Food Inspection Info  
**Shape:** (50000, 17)

*Each row represents one business in Chicago.*

Columns and types:  
Inspection_Id - int64  
dba _name - object  
Aka_name - object  
License_ - float64  
Facility_type - object  
risk- object  
address- object  
city - object  
state - object  
Zip - float64  
inspection_date - object  
Inspection_type - object  
results - object  
Violations - object  
Latitude - float64  
Longitude - float64  
Location - object  

*Each row represents one License of a Chicago Business*  
**Shape:** (50000, 37)  

Important columns and type:  
Id - object  
License_id - int64  
Account_number - int64  
Site_number - int64  
Legal_name - object  
Doing_business_as_name - object  
Address - object  
City - object  
State - object  
Zip_code - object  
Ward - float64  
Precinct - float64  
Ward_precinct - object  
Police_district - float64  
Community_area - float64  
Community_area_name - object  
Neighborhood - object  
License_code - int64  
License_description - object  
Business_activity_id - object  
Business_activity - object  
License_number - int64  
Application_type - object  
Application_created_date - object  
Application_requirements_complete - object  
Payment_date - object  
Conditional_approval - object  
License_start_date - object  
Expiration_date - object  
License_approved_for_issuance - object  
Date_issued - object  
License_status - object  
License_status_change_date - object  
Ssa - float64  
Latitude - float64  
Longitude - float64  
Location - object  

Date range in our sample: January 11, 2024 - September 25, 2026

The Chicago Food Inspection dataset provides License information and violation reports for Illinois restaurants. The two files are linked using **License_Id**


[American Community Survey (ACS)](https://www.census.gov/programs-surveys/acs/data/data-via-api.html)

We may use ACS data later to provide neighborhood-level demographic and
economic controls such as household income, employment, population, and
poverty. Restaurant coordinates could be connected to Census geographic areas
and compared with these neighborhood characteristics.


## Unit of Analysis

The main unit of analysis will be the **restaurant-month**.

For each restaurant, reviews will be grouped by month. Review characteristics from an earlier observation period will then be used to predict business outcomes during a later period.

## Variables

The main independent variables will be constructed from online review data.

Potential variables include:
- Average star rating
- Change in average rating over time
- Number of reviews per month
- Growth or decline in review volume
- Percentage of one-star reviews
- Percentage of five-star reviews
- Standard deviation of ratings
- Average sentiment of review text
- Change in sentiment over time
- Frequency of negative reviews
- Frequency of complaints related to service, food quality, cleanliness, price, wait times, or staffing
- Sudden increases in negative reviews
- Reviewer activity or experience when available
