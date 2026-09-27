# CS418_Economic_Trajectories_Group
### Research Question: What characteristics of online reviews best predict future changes in a restaurant or local business's sales, popularity, or likelihood of closure?

### Primary datasets:
- [DOI Yelp Open Dataset](https://business.yelp.com/data/resources/open-dataset/?utm_source=chatgpt.com)
- [Google Local Reviews Dataset: UCSD](https://cseweb.ucsd.edu/~jmcauley/datasets.html?utm_source=chatgpt.com)

### Secondary datasets:
- [Chicago Business Licenses](https://data.cityofchicago.org/Community-Economic-Development/Business-Licenses/r5kz-chrr/about_data)
- [Chicago Food Inspections](https://data.cityofchicago.org/Health-Human-Services/Food-Inspections/4ijn-s7e5/about_data)
- [American Community Survey (ACS)](https://www.census.gov/programs-surveys/acs/data/data-via-api.html?utm_source=chatgpt.com)

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