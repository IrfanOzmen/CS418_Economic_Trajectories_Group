# Checkpoint Four: Data Documentation
## 1. Write a datasheet for each primary dataset.
### Princeton Longitudinal Yelp Reviews Dataset (Chicago)

<img src="CS418_Checkpoint_3_Princeton%20Longitudinal%20Yelp%20Reviews%20Dataset%20%28Chicago%29.png" alt="Princeton Longitudinal Yelp Reviews Dataset nutrition label" width="50%">

### Google Local Reviews Dataset

<img src="CS418_Checkpoint_3_Google%20Local%20Reviews%20Dataset.png" alt="Google Local Dataset nutrition label" width="50%">



## 2. Define Scope: Target Population, Access Frame, and Sample**

#### Question 1: Is review volume still associated with permanent closure after accounting for restaurant age, category, and chain status?

* Target Population: Restaurants in Chicago, both open and closed from 2018 to 2021
* Access Frame: Unique Chicago restaurants (8,034) from Google Local Business Metadata. Age from the Chicago Business Licenses dataset
* Sample: 5,429 restaurants: 3,989 operating and 1,440 permanently closed. We ignore 292 temporarily closed and 2,313 with missing status.
* Missing/Limits: We have 2,313 restaurants with missing status, which might have an impact on closure rate conclusions we make. Additionally Google gives a snapshot with no closure date which makes it difficult to conclude when the restaurant closed after the inspection.

#### Question 2:Do the content of written reviews, not just star ratings, signal worse inspection outcomes?

* Target Population: Restaurants in Chicago that were inspected from 2018 to 2021
* Access Frame: 38,360 restaurants inspected in date range 01/02/2018 and 12/30/2021 (Pass: 15351, Fail: 8446). 22,831 of inspections were linked to Google restaurants, which gives us 5,182 restaurants.
* Sample: 4,838 inspected restaurants with at least five Google ratings in the year before inspection
* Missing/Limits: There are 29958 reviews with no text and those can't be used for this question but it's a large percentage. We lean toward more active restaurant because we look at ones that had at least five Google ratings in yers before inspection.

#### Question 3: Do more detailed inspection measures (violation severity, risk level, repeat failures) help explain permanent closure beyond review volume

* Target Population: Restaurants in Chicago from 2018 to 2021
* Access Frame: 5,182 unique linked Google restaurants (from Google and inspection data)
* Sample: 3,251 restaurants with know open/permanetly closed status
* Missing/Limits: As mentioned before we don't know exact closure dates. There is also an impact of pandemic in 2020 which affects certain data, so we should focus on inspections before vs after that year in order to avoid any limitations.
## 3. Write a codebook.
### Economic Trajectories Codebook:
Note: We are no longer using prior Yelp Dataset, instead will begin using Princeton Yelp Reviews Dataset (Chicago). As we go forward and begin using we will add columns to codebook.

| Variable | Data type | Units | Meaning | Range / Allowed values | Missing-value coding | Origin | Table |
|---|---|---|---|---|---|---|---|
| gmap_id | object | None but this is the unique identifier | This is a unique Google Maps ID identifier of the business. With this we are able to link google reviews specific to the matches | 0x...:0x... Unique after dropping 9 duplicate rows from the Chicago restaurant subset (1,001 duplicate rows in the full Illinois file). | None missing | meta-Illinois.json.gz, columngmap_id review-Illinois.json.gz, columngmap_id | google_metadata & google_reviews |
| name | object | None | The actual name of the restaurant as it is shown on google maps | Free text | None missing | meta-Illinois.json.gz | google_metadata |
| address | object | None | The full address in the format of (restaurant name, street, city, state, and zip code) | Chicago rows match, Chicago, IL | 2163 of 179,205 Illinois rows, none among the Chicago restaurants because the Chicago filter needs an address | meta-Illinois.json.gz | google_metadata |
| latitude | float64 | degrees north | Latitude of the inspected business. Used to match to Google restaurants | kept only if between 41.64 and 42.10 | 41 of the 38,401 rows the query returns, those rows are dropped but none fell outside the Chicago bounding box | Chicago Data Portal, latitude | food_inspections |
| longitude | float64 | degrees east | Longitude of the inspected restaurant | -87.91 to -87.53 (kept only if between -87.95 and -87.50) | Rows with no coordinates are dropped (see latitude) | Chicago Data Portal, longitude | food_inspections |
| category | object | None | This shows category labels, for example we see “Restaurant, American restaurant, Bar” and we filter out for “Restaurants” , any label containing “restaurant" will be filtered | 1 to 11 labels per Chicago restaurant | 719 of 179,205 Illinois rows; those rows cannot be classified, so none are in the restaurant subset | meta-Illinois.json.gz, category tested with has_restaurant_category() | google_metadata |
| avg_rating | float64 | Stars | Average Google star rating | 1.0 to 5.0 | None missing | meta-Illinois.json.gz, avg_rating | google_metadata |
| num_of_reviews | int64 | None | Total number of Google reviews at the time | 1 to 9998 (max) | None missing | meta-Illinois.json.gz, num_of_reviews | google_metadata |
| time | int64, but converted to datetime64[ns] | milliseconds since 1970-01-01 UTC (raw) | When the review was posted and the prior review window compares it with the inspection date | 2008-01-29 to 2021-09-08 | None missing in the first 100,000 rows; rows whose time or rating is not numeric are skipped in the review scan | review-Illinois.json.gz (23096838 rows) | google_reviews |
| rating | int64 | Stars | Reviewers star rating for the business | 1, 2, 3, 4, 5 (mean 4.42 in the first 100,000 rows) | None missing | review-Illinois.json.gz, rating | google_reviews |
| user_id | float64 | None | Anonymous reviewer ID we used with time in google_reviews table to look for duplicate reviews (20,204 repeated user_id + time rows in the first 100,000) | About 1.0e20 to 1.18e20 (first 100,000 rows) | None missing in the first 100,000 rows | review-Illinois.json.gz (23096838 rows), user_id duplicate check | google_reviews |
| inspection_id | int64 | None | Unique ID of one inspection | 38360 rows kept (restaurants, 2018-2021) | None missing | Chicago Data Portal, inspection_id | food_inspections |
| dba_name | object | none | "Doing business as" name of the inspected business which is used to match to Google by name | Free text | None, the rows with a missing name are dropped | Chicago Data Portal, dba_name | food_inspections |
| address | object | none | Street address of the inspected business | Free text | None missing | Chicago Data Portal, address | food_inspections |
| facility_type | object | none | The kind of facility, we filtered and keep exactly "Restaurant" | "Restaurant" (the other types are filtered out in the query) | None missing after filtering | Chicago Data Portal, facility_type filter to Restaurant | food_inspections |
| inspection_date | datetime64[ns] object | date | The date of the inspection which restaurant had | 2018-01-02 to 2021-12-30 | Rows with a missing date are dropped (none found) | Chicago Data Portal, inspection_date, parsed with pd.to_datetime | food_inspections |
| results | Category, ordered | none | Outcome/results of the inspection (pass, pass w/conditions, or fail) | Ordered Pass < Pass w/ Conditions < Fail: Pass 15,351, Pass w/ Conditions 14,563, Fail 8,446. The query leaves out Out of Business, No Entry, Not Ready and Business Not Located. | None missing | Chicago Data Portal results, filtered and set with pd.Categorical(.., ordered=True) | food_inspections |
| status_group | object | None | Give status group of filtered restaurants in chicago (observed operating, permanently closed, temp closed, or missing status) | "Observed operating" 3,989, "Permanently closed" 1,440, "Temporarily closed" 292, ”Missing status" 2,313 (8,034 restaurants) | No missing status is its own level | Derived: np.select(…) on state "Observed operating" means the status is an opening hours message | chicago_restaurants |
| permanently_closed | int64: 0 or 1 | None | **Outcome variable.** 1 if Google says the restaurant is permanently closed, 0 if it was observed operating. | 0 (3,989) or 1 (1,440); 26.5% closed among the 5,429 restaurants kept | Not defined for temporarily closed or missing-status restaurants, the 2605 are dropped before this column is made | Derived: status_group == "Permanently closed”).astype(int) | analysis_restaurants |
| rating_band | Ordered category | stars | avg_rating grouped into bands for the “Permanent closure percentage by Google rating and review volume” heatmap | "<3.0" 109; "3.0-3.4" 278; "3.5-3.9" 1,001; "4.0-4.4" 2,407; "4.5-5.0" 1,634 | avg_rating has no missing values | Derived: pd.cut(avg_rating, bins=[-inf, 3.0, 3.5, 4.0, 4.5, inf], right=False) | analysis_restaurants |
| review_volume_quartile | Ordered category | reviews | Quartile of num_of_reviews among the analysis restaurants | Q1 (1-46 reviews, 1,376 restaurants); Q2 (47-119, 1,339); Q3 (121-298, 1,394); Q4 (302-9,998, 1,320) | None | Derived:pd.qcut(num_of_reviews, q=4, labels=[“Q1","Q2","Q3","Q4"]) (Relationship Plot 1) | analysis_restaurants |
| normalized_name | object | None | Business name lowercased, accents removed, punctuation and words like "inc", "llc", "the" and "restaurant" dropped which is used to compare names | Free text | None missing | Derived: normalize_business_name(name or dba_name) | google_match_table, inspection_match_table |
| street_number | object | None | First whole number found in the address that is used as a backup for matching | Digits | Not available | Derived: extract_street_number(address) | google_match_table, inspection_match_table |
| grid_lat | int64 | 0.001 degrees | Latitude cell used to find nearby candidates quickly | 28647 to 42020 (the 28647 comes from the two placeholder Google rows) | None missing | Derived: floor(latitude * 1000)` | google_match_table, inspection_match_table |
| grid_lon | int64 | 0.001 degrees | Longitude cell used to find nearby candidates quickly | -87911 to 180000 (the 180000 comes from the two placeholder Google rows) | None missing | Derived: floor(longitude * 1000) | google_match_table, inspection_match_table |
| inspection_name | object | None | Copy ofdba_name, kept beside each match so the pairs can be checked | Free text | None in matched rows | Derived: copied from dba_name | matched_inspections |
| google_name | object | None | Copy of the Googlename of the matched restaurant, kept for the same reason | Free text | None in matched rows | Derived: copied fromname | matched_inspections |
| distance_m | float64 | Meter | Straight line distance between an inspection and its matched Google restaurant | 0.07 to 59.78 (median 13.9) | None in matched rows, the 15529 unmatched inspections are dropped | Derived: haversine_distance_m() | matched_inspections |
| name_similarity | float64 | Ratio, 0.00 to 1.00 | How alike the two cleaned names are similarity (difflib.SequenceMatcher) | 0.22 to 1.00 (median 1.00). Accepted if at least 0.80 within 60 m, or within 40 m with the same street number and at least 0.55 (or one name inside the other). 1325 matches are below 0.55 | Never in matched rows | Derived: difflib.SequenceMatcher(None, name_a, name_b).ratio() | matched_inspections |
| match_score | float64 | None | name_similarity - distance_m / 1000 The highest score per inspection wins | 0.20 to 1.00 | Never in matched rows | Derived: `name_similarity - float(distance_m) / 1000 the best score per inspection is kept with sort_values(...).drop_duplicates("inspection_id" | matched_inspections |
| negative_review_percentage | float64 | Percent | negative_review_share *100 | 0 to 100 | Never in kept rows | Constructing prior review feature ):100 * negative_review_share | inspection_events |
| prior_average_rating | float64 | None | Mean Google rating in the same day window | 1 to 5 | Never in kept rows | Derived Constructing prior-review features): np.mean(prior_ratings)` | inspection_events |
| volume_group | object | None | Splits restaurants at the median Google review count of the Plot 3 sample | "At or below median (188 reviews)" or "Above median" (3,251 restaurants) | Never | Derived: np.where(num_of_reviews <= median, low_label, high_label) (Relationship Plot 3) | plot3 |

## 4. Share the recipe.

[View Recipe Notebook](Recipe.ipynb)

## 5. Document access and reuse.

The following checklist goes over the FAIR principles.

### Princeton Longitudinal Reviews

**Access:** [dataset page](https://sites.google.com/princeton.edu/longitudinal-review-data). Access requires a detailed email to the publishers of the data explaining how will the data be used.

**Interoperable format:** We downloaded this dataset on **October 27, 2026**. No formal dataset version is stated.

**Terms and reuse of processed data:** The [arXiv record](https://arxiv.org/abs/2202.09005) links to CC BY 4.0. This also covers the dataset. Under this others can also use our cut Chicago only extraction.

### Google Local Data (2021)

**Access:** [dataset page](https://jiachengli1995.github.io/google/index.html). The Illinois [reviews](https://mcauleylab.ucsd.edu/public_datasets/gdrive/googlelocal/review-Illinois.json.gz) and [business metadata](https://mcauleylab.ucsd.edu/public_datasets/gdrive/googlelocal/meta-Illinois.json.gz) are available on UCSD website.

**Interoperable format:** We downloaded the Illinois files on **September 27, 2026**. The 2021 release covers reviews through September 2021.

**Terms and reuse of processed data:** The creators request citations but is no stated redistribution permission specified. Therefore unconfirmed but we can still share processing code, schema, and publish results and link users to the source files under Google Maps terms and third-party content.

### Chicago Food Inspections

**Access:** [dataset page](https://data.cityofchicago.org/d/4ijn-s7e5), available publicly.

**Interoperable format:** We downloaded the CSV sample on **September 27, 2026**. This is a regularly updated feed with a stable Socrata dataset ID.

**Terms and reuse of processed data:** The portal lists its license as "See Terms of Use." We can use [City's data terms](https://www.chicago.gov/city/en/narr/foia/data_disclaimer.html).

### Chicago Business Licenses

**Access:** [dataset page `r5kz-chrr`](https://data.cityofchicago.org/d/r5kz-chrr), available publicly.

**Interoperable format:** We downloaded the CSV sample on **September 27, 2026**. This is a regularly updated feed with a stable Socrata dataset ID [JSON and other exports](https://catalog.data.gov/dataset/business-licenses) are available.

**Terms and reuse of processed data:** The [City of Chicago Data Terms of Use](https://www.chicago.gov/city/en/narr/foia/data_disclaimer.html) apply. Everyone else can reuse a city only dataset subject to these same terms.


## 6. Consider any people in the data. 

### Representation of People:

Both datasets capture data from people who have submitted online reviews for businesses.

In the Google Local Reviews dataset, there are identifiers for each reviewer, their name, rating, comments and potentially photos. In the Princeton Longitudinal Reviews dataset, there are customer ratings, comments, whether they would recommend the business and information about the reviewers such as if they were ‘elite’, how many friends they had and how many reviews they posted. Some personal details were removed from the dataset such as the reviewer names, profile links and images.

This does not form a random, representative sample of the population. Those who chose to use online review sites may not be representative of everyone who uses a restaurant.

Those who don’t use online review tools, have limited internet access or are uncomfortable leaving reviews may not be well represented. The Princeton dataset only includes reviews for restaurants in Chicago whereas the Google Local Reviews dataset includes businesses in Illinois.

### Biases:
Those who have left reviews may not be representative.
The Princeton dataset only includes reviews for restaurants in Chicago, and the Google Local Reviews dataset only includes businesses from Illinois. This means any conclusions from the datasets may not be generalisable to other cities and states.
Popular businesses may be overrepresented as they will have more reviews.
Those who have very good or bad experiences may be more likely to leave a review.
The Princeton dataset has categories for recommendations, non-recommendations and reviews that have been removed. This may create further bias.
Only 32 businesses were labelled as closed in the Princeton dataset (out of 5,355). This creates a large class imbalance.
Both datasets are historical so changes in customer behaviours, restaurant behaviours and online review behaviours may mean they are not relevant anymore.

### CARE:

**Collective benefit** The research should ensure that it helps understand restaurant trends and find meaningful patterns. It should not disadvantage any particular businesses or communities.

**Authority to control** Those who have left reviews, those who own the business and those in the community may be interested in how this data is collected, shared and analysed. Researchers should follow any restrictions on how the data can be used and recognise that just because the reviews are available to the public does not mean they have given permission for them to be used.

**Responsibility** Researchers should be aware of any limitations within the data. Check if the data is representative geographically and demographically. Be aware of any biases. Do not make any unsupported claims about individual restaurants.

**Ethics** The analysis should be respectful of the reviewers privacy. Recognise that the data may not be fully representative. Do not make any conclusions that would unfairly stereotype businesses or communities. Although some personal details have been removed from the Princeton dataset, there may still be identifying details in the comments and other attributes.

### Those who may be Impacted:
Those who own the restaurant, work there, use it or live in the community may be impacted.
For example, if a model predicted that a restaurant was going to close it may incorrectly predict that a successful business is at risk. This could harm the reputation of the business if the predictions were not shown with the correct level of uncertainty.
Restaurants in areas where people don’t leave as many online reviews may be unfairly stereotyped as not being as popular.
For this reason, predictions should be seen as estimates. The analysis should be transparent, fair and interpreted responsibly.
