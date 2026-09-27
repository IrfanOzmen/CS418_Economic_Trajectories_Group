# CS418_Economic_Trajectories_Group
### Research Question: What characteristics of online reviews best predict future changes in a restaurant or local business's sales, popularity, or likelihood of closure?

## Primary datasets:

### 1. Yelp Open Dataset
[DOI Yelp Open Dataset](https://business.yelp.com/data/resources/open-dataset/?utm_source=chatgpt.com)
#### Yelp Business Data

- **Shape:** 150,346 rows × 14 columns
- **One row represents:** One business listed in the Yelp Open Dataset.
- **Important columns:**
  - `business_id` — object/string — unique business identifier
  - `name` — object/string — business name
  - `address` — object/string — street address
  - `city` — object/string — city
  - `state` — object/string — state or region
  - `latitude` — float — business latitude
  - `longitude` — float — business longitude
  - `stars` — float — average Yelp rating
  - `review_count` — integer — number of reviews
  - `is_open` — integer — indicator for whether the business is listed as open
  - `categories` — object/string — business categories
- **Geographic coverage:** Multiple U.S. and Canadian regions are represented
  in the loaded business data.
- **Intended use:** The business file provides restaurant characteristics,
  location, rating, review count, and open/closed status. `business_id` allows
  it to be joined with Yelp reviews.
#### Yelp Review Data

- **Checkpoint sample:** 100,000 rows × 9 columns
- **One row represents:** One Yelp review written for one business.
- **Important columns:**
  - `review_id` — object/string — unique review identifier
  - `user_id` — object/string — reviewer identifier
  - `business_id` — object/string — identifies the reviewed business
  - `stars` — integer — rating given by the reviewer
  - `text` — object/string — written review
  - `date` — datetime — date and time of the review
  - `useful` — integer — useful-vote count
  - `funny` — integer — funny-vote count
  - `cool` — integer — cool-vote count
- **Time coverage of our 100,000-review sample:** March 1, 2005 to October 4,
  2018.
- **Intended use:** We will use review ratings, text, and dates to construct
  features such as average ratings, changes in ratings, review volume, and
  review sentiment.

The loaded Yelp review sample contains 100,000 observations and 9 columns.

### 2. Google Local Reviews Dataset (UCSD)
[Google Local Reviews Dataset: UCSD](https://cseweb.ucsd.edu/~jmcauley/datasets.html?utm_source=chatgpt.com)

We downloaded the Illinois business metadata and review files. Because the
Illinois review dataset is very large, we loaded the complete Illinois business
metadata and a 100,000-review sample.

#### Google Local Illinois Business Metadata

- **Shape:** 179,205 rows × 15 columns
- **Restaurant subset:** 28,809 rows × 15 columns
- **One row represents:** One local business in Illinois.
- **Important columns:**
  - `gmap_id` — object/string — unique Google Maps business identifier
  - `name` — object/string — business name
  - `address` — object/string — business address
  - `latitude` — float
  - `longitude` — float
  - `category` — object/list — categories assigned to the business
  - `avg_rating` — float — average Google rating
  - `num_of_reviews` — integer — number of Google reviews
  - `price` — object/string — price level when available
  - `state` — object/string — operating status or hours/status information
- **Geographic coverage:** Illinois
- **Intended use:** We filtered the business metadata to businesses whose
  categories contain the word `restaurant`. This produced a subset of 28,809
  Illinois restaurants that can later be compared with Chicago government data.

The full Illinois metadata contains 179,205 businesses, of which 28,809 met
our restaurant-category filter.

#### Google Local Illinois Reviews

- **Checkpoint sample:** 100,000 rows × 8 columns
- **One row represents:** One Google review of one Illinois local business.
- **Important columns:**
  - `gmap_id` — object/string — joins a review to Google business metadata
  - `rating` — integer — rating given by the reviewer
  - `text` — object/string — review text
  - `time` — integer — review timestamp stored in milliseconds
  - `resp` — object — business response when available
  - `pics` — object — attached picture information when available
- **Time coverage of our sample:** January 29, 2008 to September 8, 2021.
- **Geographic coverage:** Illinois
- **Intended use:** This dataset will provide the main review-level predictors
  for the Illinois portion of our analysis. `gmap_id` can be used to link each
  review to the Google business metadata.

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

The loaded license sample contains 50,000 rows and 37 columns, including
business names, license information, neighborhood information, dates, and
coordinates.

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

The inspection dataframe contains 50,000 rows and 17 columns, including
restaurant name, inspection date, inspection result, violations, license
number, and coordinates.

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
