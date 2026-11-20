# CS418_Economic_Trajectories_Group
Group members: Irfan, Briana, Masa, Harsh
### Research Question: What characteristics of online reviews best predict future changes in a restaurant or local business's sales, popularity, or likelihood of closure?

Revised: What characteristics of restaurants, such as proximity to similar businesses, review volume, ratings, review negativity score, and how long a business stayed open, help predict a restaurant's likelihood of closure?

## Main Findings (Narrative Analysis)

- Finding 1: Google marked 52.0% of restaurants in the lowest review-count quartile as permanently closed, versus 7.0% in the highest.
- Finding 2: Restaurants that failed averaged 12.5% one- or two-star Google ratings in the year before inspection, versus 10.6% for restaurants that passed.
- Finding 3: Google marked 16.4% of restaurants that failed their first qualifying inspection as permanently closed, versus 15.7% of those that passed.

### Finding 1: 

In the Google dataset which contained 5,429 Chicago restaurants had known closure status. Those restaurants that were in the lowest review count quartile: 52.0% of them were marked permanently closed versus only 7.0% in the highest, a gap of 45.1 percentage points. Restaurants that stay open longer can build up more reviews. On the same 1,164 matched restaurants, the model estimated a 50.7-point gap at 18 versus 518 reviews before adjustment and 57.5 points after adding the time model since the first recorded license. This license date issue data was acquired from the Chicago Business Licenses [https://data.cityofchicago.org/Community-Economic-Development/Business-Licenses/r5kz-chrr/about_data] database. For the restaurant, the gap was 58.6 points. Adding community area gave a 64.7% versus 6.7%, a 57.9-point gap (95% CI 49.7 to 66.2). So, a slightly lower gap than before the community area was added. Using the first Google Review data as the age proxy gave a 62.6- point gap. The exact-name license check gave a 59.4-point gap. These estimates describe closure status as recorded by Google. Differences in actual operating lifetimes could still contribute to the gap because opening and closure dates are unknown.

<img width="1196" height="559" alt="image" src="https://github.com/user-attachments/assets/805fa5de-6303-4122-8262-8a7e86b5b10d" />

Later, in the Princeton Yelp dataset, with 4,984 having a dated review history and 32 labeled as closed, we found that once the age of the first review was controlled for, the relationship between review counts and closure became weaker, with the odds ratio being reduced from 1.516 to 1.261. On the other hand, calculating the odds ratio using the time between the first and last reviews resulted in 1.631 odds ratio. These results imply that the period of a restaurant’s review history is one of the factors responsible for the relationship under study.

<img width="1192" height="602" alt="image" src="https://github.com/user-attachments/assets/fe3e4fb2-a9b2-4ce5-a7ab-c59c88fa0a3c" />

However, later investigation found that all the closed restaurants in the Princeton Yelp data came from a single group. How the data was gathered showed that restaurants that were closed were only gathered from the EYG (Eight Year Gap) group that were 123 restaurants that were specifically picked for examination between years 2012 and 2020. All of the 32 restaurants that were marked closed in the Princeton Yelp data came from this group. This was an issue because, when gathering the other groups (CHI and UDIS), permanently closed restaurants were not included. So we decided to test our findings with the EYG group, which would not have bias towards closed restaurants.

When the EYG group data was taken under investigation, we saw that 91 of the open restaurants had a median review count of 1,247. When we compare this to the median of 398 reviews among those 32 closed restaurants, the story became clearer. Of all the 121 complete restaurants (2 restaurants’ closure status was unknown), adding observed review age reduced the estimated low vs. high reviews gap from 52.4 to 47.9 percentage points, so a very small drop, as we saw in the Google database. When we added our rating, negative-share, and community area controls, the fitted probability of closure between restaurants with 72 reviews and 2245 reviews was found to be  59.3% and 11.1%, a sizable 48.1-point gap. We also used a cluster-bootstrap 95% interval of 24.5 to 70.3. The main adjusted result has the same direction as the Google finding. We also saw that this direction was unaffected by the leave-one-area-out checks.

<img width="1248" height="611" alt="image" src="https://github.com/user-attachments/assets/da26c3be-4130-4fa3-8b17-28f6fdd9a157" />


### Finding 2: 

Negative ratings & inspection results 

From this we were able to find that restaurants that failed their first inspection had slightly more 1 or 2 star Google ratings in the year before their inspection than restaurants that passed, 12.6% compared to 10.7% which we had found in EDA. The difference was about 1.8 percentage points . Since the interval does not include zero it is pretty small but it is significant. We also checked whether our results changed when using different review periods and minimum numbers of ratings. The gap stayed between 1.7 and 2.3 percentage points, and for high risk restaurants it was 1.9 points. Showing small connection between negative Google ratings and inspection results but the ratings itself isn't enough to tell whether a restaurant will fail. This also does not mean that negative ratings cause inspection failures.

<img width="1226" height="663" alt="finding2" src="https://github.com/user-attachments/assets/4449d29f-70af-4c67-9681-ab895ee2410b" />

### Finding 3:

Restaurants that failed their first inspection were marked permanently closed 16.4% of the time, compared with 15.7% of those that passed, making the gap between them too small to treat inspection results as a strong reason for closure and put focus on the Google reviews that show that restaurants with a number of reviews above the median are less likely to be marked permanently closed regardless of the inspection pass/fail results.

In the sample of 3251 restaurants with know open/permanently closed status we were looking at, 16.4% of those who failed the inspection were marked permanently closed and 15.7% passed, creating a gap of 0.7 percentage points. However, in our calculation of the 95% confidence interval, we found that the confidence interval for this gap between restaurants that failed and passed inspections that were permanently closed ranges from -2.7 to 4.1 percentage points. This means that we can't confidently conclude that restaurants that fail or pass the inspection are more likely to be permanently closed, and therefore the inspection results are not clearly associated with whether the retsaurants will be marked permanently closed

<img width="687" height="435" alt="image" src="https://github.com/user-attachments/assets/2d89ac2a-9ed6-48f1-a73b-8c6bfd9f3bb2" />

The graph shows the relationship we expected. Two lines are model's predicted chance of closure based on whether the restaurant failed or passed their first inspection. These lines are very close to each other and the orange line shows that restaurants that fail their first inspection have a slightly higher odds of being marked closed (1.1). As number of google reviews increases, predicted chance of being marked closed decreaseses and both lines follow the downward slope.

In our calculation of the 95% confidence interval, we found that the odds ratio ranges from 0.85 to 1.42. Since the range can be below 1, above 1 or 1, we can't confidently conclude that failing the first inspection increases, decreases or is linked at all to odds of closing the restaurant, meaning we can't conclude that failing an inspection is linked to the closure of the restaurant.
Pass w/condition check
Out of 3251 restaurants with known open/permanently closed status, 1447 of them had the result Pass w/Condition. Without considering this group we have our finding that restaurants that failed their first inspection were marked permanently closed 16.4% of the time, compared with 15.7% of those that passed, but we want to check if counting this group as either pass or fail would change our finding and make the gap more significant.

We can see that around 30% of restaurants that failed their first inspection were removed and around 36% of those that passed, which is slightly more. The gap is -5.3 and CI ranges from -8.87 to -1.78, which doesn't include 0. Looking at this we can conclude that there was more restaurants that passed that were excluded from the analasys. We marked this as a limitation and don't think that this changes our finding because of the small difference in percentage and the previous results we got from different checks.

## Primary datasets:

### 1. Princeton Longitudinal Yelp Reviews Dataset (Chicago)

The [Princeton Longitudinal Reviews Dataset](https://sites.google.com/princeton.edu/longitudinal-review-data) replaces the Yelp Open Dataset for Chicago analysis. The encrypted source archive is `data/longitudinal_reviews_2022-05-25-1444.zip`; it is ignored by Git. The processed Chicago restaurant data is in `data/processed_princeton_chicago/`:

* `businesses.parquet`: one row per unique Chicago restaurant, with its Yelp business ID, name, address, coordinates, categories, rating, closure flag, and a JSON column containing all retained original business fields.
* `reviews.parquet`: one row per deduplicated review, linked by `business_id`. It includes review text, date, rating, reviewer metrics, and recommendation status.
* `manifest.json`: row counts, included source crawls, and processing rules.

The conversion reads the latest CHI and UDIS crawls plus EYG directly from the encrypted ZIP. It selects `Chicago, IL` businesses using Yelp's Restaurants category hierarchy. Only reviewer names, reviewer profile URLs, reviewer image URLs, and business image URLs/photos are removed. Business names and Yelp business URLs remain for matching to the Google Local data.

The output has **5,355 restaurants** and **1,331,020 deduplicated reviews**. Of those restaurants, 4,984 have a review in the selected snapshots.

To rebuild the Parquet files, install `pyarrow` and run `python scripts/process_princeton_chicago.py`; enter the ZIP password at the prompt. The script never stores the password. Output files must not already exist. Notebook readers need `pandas` and `pyarrow`. The current workspace has `pyarrow` in `data/.python_deps`; install it in your own environment if that folder is unavailable.

In a notebook:

```python
from pathlib import Path
import sys
import pandas as pd

data_dir = Path("data/processed_princeton_chicago")
sys.path.insert(0, str(Path("data/.python_deps").resolve()))
businesses = pd.read_parquet(data_dir / "businesses.parquet")
reviews = pd.read_parquet(data_dir / "reviews.parquet")
# Some original dates end with "Updated review"; preserve the raw date
# and parse its leading month/day/year for time-series analysis.
reviews["review_date"] = pd.to_datetime(
    reviews["date"].str.extract(r"^(\d{1,2}/\d{1,2}/\d{4})", expand=False),
    format="%m/%d/%Y",
)
restaurant_reviews = reviews.merge(
    businesses[["business_id", "name", "city", "state", "category_aliases"]],
    on="business_id",
    validate="many_to_one",
)
```

### Yelp Open Dataset (legacy; no Chicago coverage)
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

**Each row represents one business in Chicago.**

**Important columns and types** 
* Inspection_Id - int64  
* dba _name - object  
* Aka_name - object  
* License_ - float64  
* Facility_type - object  
* risk- object  
* address- object  
* city - object  
* state - object  
* Zip - float64  
* inspection_date - object  
* Inspection_type - object  
* results - object  
* Violations - object  
* Latitude - float64  
* Longitude - float64  
* Location - object

 **Time**: January 11, 2024 - September 25, 2026

### 4. Chicago Business Licenses
[Chicago Business Licenses](https://data.cityofchicago.org/Community-Economic-Development/Business-Licenses/r5kz-chrr/about_data)

**Shape:** (50000, 37)

**Each row represents one business-license on record**

**Important columns and their types**
* Id - object  
* License_id - int64  
* Account_number - int64  
* Site_number - int64  
* Legal_name - object  
* Doing_business_as_name - object  
* Address - object  
* City - object  
* State - object  
* Zip_code - object  
* Ward - float64  
* Precinct - float64  
* Ward_precinct - object  
* Police_district - float64  
* Community_area - float64  
* Community_area_name - object  
* Neighborhood - object  
* License_code - int64  
* License_description - object  
* Business_activity_id - object  
* Business_activity - object  
* License_number - int64  
* Application_type - object  
* Application_created_date - object  
* Application_requirements_complete - object  
* Payment_date - object  
* Conditional_approval - object  
* License_start_date - object  
* Expiration_date - object  
* License_approved_for_issuance - object  
* Date_issued - object  
* License_status - object  
* License_status_change_date - object  
* Ssa - float64  
* Latitude - float64  
* Longitude - float64  
* Location - object  

**Time**: February 16, 2004 - July 16, 2028

[American Community Survey (ACS)](https://www.census.gov/programs-surveys/acs/data/data-via-api.html)

We could use ACS data later to provide neighborhood demographic information.


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


## Exploratory analysis

### Dataset overview

We examined four main datasets. The Yelp Business dataset contains 150,346 businesses. The Yelp review sample contains 100,000 reviews. The Google Local Business Metadata contains 179,205 Illinois businesses, including 28,809 businesses categorized as restaurants, and our initial Google Review profile uses a 100,000-review sample. 

While doing the relationship analyses, we filtered the Google data to Chicago restaurants and joined it with the Chicago Food Inspections dataset from 2018 to 2021.

The Yelp distributions describe restaurant ratings and review activity more generally. The three relationship analyses focused specifically on Chicago restaurants.

### Revised research questions

The exploratory data analysis led to these revised questions:
1. Is the review volume still associated with permanent closure after accounting for restaurant age, category, and chain status?
2. Does the content of written reviews, instead of rating alone out of 5, signal worse food-inspection outcomes?
3. Do more detailed inspection measures, like violation severity, risk level, and repeat failures, help explain permanent closure beyond review volume?

### Anomalies and data quality checks

Yelp Business Dataset
1. Missing values: There are 103 rows with missing categories. These will be dropped later in our analysis because we can't know if they are restaurants or some other businesses.
2. Duplicates: There are no duplicate businesses based on business_id.
3. Wrong data types: is_open is stored as an integer but should be stored as a boolean. This affects 150,346 rows and will be converted later.
4. Not enough Chicago data: There is only 1 business listed in Chicago, so this dataset does not have enough Chicago businesses for our main analysis.
Yelp Reviews Dataset
5. Missing values: No missing values were found.
6. Duplicates: No duplicate reviews were found based on review_id.
7. Data types and formatting: No wrong data types or inconsistent formatting were found.
8. Unmatched reviews: All reviews in the sample can be matched with a business in the Yelp Business dataset.
Google Metadata Dataset
9. Missing values: Missing values in description and price are left in the dataset and flagged.
10. Duplicates: There are 1,001 duplicate gmap_id entries in the metadata dataset.
11. Data types and formatting: No major wrong data types or inconsistent formatting were found.
Google Reviews Dataset
12. Missing values: There are 29,958 reviews with no review text. These are left in the dataset and flagged.
13. Duplicates: There are 20,204 reviews with the same user_id and time. These will be treated as possible duplicates.
14. Wrong data types and formats: time was stored as milliseconds and was converted to datetime format for all 100,000 sampled reviews. Other questionable data types are left and flagged for now.

### Significant findings

Yelp restaurant ratings were centered around 3.5 stars, with a mean of 3.52, median of 3.50 and a standard deviation of 0.83. Restaurant review counts were strongly right-skewed, meaning most restaurants had reletively few reviews. While a small number had a very large review total.

Review volume (amount) had the cleanest relationship with permanent closure among Chicago restaurants. Restaurants in the lowest Google review-volume quartile were marked permanently closed 52% of the time, compared with only 7.0% in the highest quartile. Restaurants rated below 3.0 also had higher closure percentage than restaurants rated from 4.5 to 5.0. However, the rating pattern was less reliable after separating restaurants by review volume.

Restaurants that failed a food inspection averaged 12.5% negative ratings during the preceding year, compared with 10.6% among restaurants that passed. However, the distributions overlapped substantially.

Inspection results were much more closely tied to permanent closure than review volume. Restaurants that failed their first inspection were marked permanently closed 16.4% of the time, compared with 15.7% of restaurants that passed. Within each inspection group, restaurants with lower review volume had much higher closure rates than those with higher review volume.


## Data documentation

The [Checkpoint 4 datasheet](Checkpoint_%234_Data_Documentation/DATASHEET.md) documents dataset composition, access, formats, and reuse.

- **Princeton Longitudinal Reviews:** [Princeton dataset page](https://sites.google.com/princeton.edu/longitudinal-review-data). Access by request. We assume the paper's [CC BY 4.0 license](https://arxiv.org/abs/2202.09005) also covers the data, although the dataset page does not state a separate license.
- **Google Local Data (2021):** [creators' dataset page](https://jiachengli1995.github.io/google/index.html). The creators request citations but is no stated redistribution permission specified. Therefore unconfirmed but we can still share processing code, schema, and publish results and link users to the source files under Google Maps terms and third-party content.
- **Chicago Food Inspections:** [City dataset](https://data.cityofchicago.org/d/4ijn-s7e5). The portal lists its license as "See Terms of Use." We can use the City's data terms.[City of Chicago Data Terms of Use](https://www.chicago.gov/city/en/narr/foia/data_disclaimer.html).
- **Chicago Business Licenses:** [City dataset](https://data.cityofchicago.org/d/r5kz-chrr). Everyone else can reuse a city only dataset subject to these same terms: [City of Chicago Data Terms of Use](https://www.chicago.gov/city/en/narr/foia/data_disclaimer.html).

The biggest limitation we have is that the Google Local dataset provides a status snapshot but it has no closure dates. Also 2,313 of 8,034 Chicago restaurants have no usable status. As a result, we can't tell when a restaurant was closed.
