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
[Chicago Business Licenses](https://data.cityofchicago.org/Community-Economic-Development/Business-Licenses/r5kz-chrr/about_data)

We obtained a 50,000-row sample directly from the City of Chicago Data Portal
API and loaded it into pandas.

- **Checkpoint sample:** 50,000 rows × 37 columns
- **One row represents:** One business-license record.
- **Important columns:**
  - `license_id` — integer — license record identifier
  - `license_number` — integer — business license number
  - `legal_name` — object/string — legal business name
  - `doing_business_as_name` — object/string — public-facing business name
  - `address` — object/string — business address
  - `license_description` — object/string — type of business license
  - `business_activity` — object/string — business activity
  - `application_type` — object/string — issue or renewal information
  - `license_start_date` — object/date
  - `expiration_date` — object/date
  - `license_status` — object/string
  - `license_status_change_date` — object/date
  - `latitude` — float
  - `longitude` — float
- **License-start-date coverage in our sample:** February 16, 2004 to
  July 16, 2028.
- **Geographic coverage:** Primarily Chicago business records. The `city`
  field in our sample also contains some records with other city/location
  values, so we may restrict the data to Chicago before analysis.
- **How we expect to use/join it:** We plan to match Google Local restaurant
  records with business-license records using business name, address, and
  geographic coordinates. License status, renewal, expiration, and status-change
  information may help us determine whether a business continued operating or
  closed.

[Chicago Food Inspections](https://data.cityofchicago.org/Health-Human-Services/Food-Inspections/4ijn-s7e5/about_data)

We obtained a 50,000-row sample directly from the City of Chicago Data Portal
API and loaded it into pandas.

- **Checkpoint sample:** 50,000 rows × 17 columns
- **One row represents:** One inspection of a food establishment.
- **Important columns:**
  - `inspection_id` — integer — unique inspection identifier
  - `dba_name` — object/string — establishment name
  - `license_` — float — Chicago license number
  - `facility_type` — object/string — type of establishment
  - `address` — object/string — establishment address
  - `inspection_date` — object/date
  - `inspection_type` — object/string
  - `results` — object/string — result such as Pass or Fail
  - `violations` — object/string — violations reported during inspection
  - `latitude` — float
  - `longitude` — float
- **Time coverage of our 50,000-row sample:** January 9, 2024 to
  September 25, 2026.
- **Geographic coverage:** Primarily Chicago. The sample also contains a small
  number of other values in the `city` field, which we may clean or filter
  before the final analysis.
- **How we expect to use/join it:** We plan to compare these inspection records
  with Google Local restaurants using establishment name, address, coordinates,
  and potentially license number after matching to the business-license data.
  Repeated inspections may also serve as evidence that a restaurant remained
  in operation at a later date.

[American Community Survey (ACS)](https://www.census.gov/programs-surveys/acs/data/data-via-api.html?utm_source=chatgpt.com)

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
