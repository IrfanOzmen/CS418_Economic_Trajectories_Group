

## Section 1 and Section 2: Profile each primary dataset and Make a note of anomalies:

Section 1 goes through each primary dataset and reports the most important information about them and things that stand out. For each dataset we reported its shape, its types, the number of missing value and summary statistics for it. For all of them we wrote a short paragraph of what stood out to us. Section 2 focuses discovering missing values, duplicated, inconsistent formatting, wrong data types, invalid uses and mixes units for all our primary datasets.

1) The Yelp Business dataset is 150346 rows and 14 columns . In this dataset is that some columns have a different datatype from what is expected is_open is stored as a number even though it should be yes or no boolean. The column attribute’ is missing 13744 rows and ‘hours’ is missing 23223 rows, which is more than the rest of the columns. Looking at the latitude and longitude in the summary table we can see that the dataset contains the data for states other than IL too, so it's worth checking if it contains enough data that we can use for IL businesses. Looking at the mean of ‘is_open’ we can see that about 20% of the restaurants are closed. We also noticed that ‘review_count’ is right skewed since its mean(44.8) is greater than its median (15).
* Missing values will be drop later in our analysis because we can't know if they are restaurants or some other bussinesses. Rows affected 103
* We checked if there are any duplicate businesses in the dataset and droped them. Rows affected: none
* Wrong data types: 'is_open' should be stored as boolean instead of integer, convert it. Rows affected: 150346

2) The Yelp Review Database currently has 100000 rows which we limited it to and 9 columns. Each row represents one review for one business. All the columns have correct datatypes and there are no missing values. Looking at the statistics table we can see that the date range for these reviews goes from 03/01/2005 to 04/10/2018, which is useful to note. Starts have a minimum of 1 and maximum of 5 which seems usual. ‘Useful’, ‘funny’, ‘cool’ reviews are right skewed.
* No missing values.Rows affected: none
* We checked if there are any duplicate reviews in the dataset and droped them. Rows affected: none
* No wrong data types, or inconsistent formatting
* We checked if there are reviews that can't be matched with business. Row affected: none since everything matches.

3) The Google Local Business Metadata has 179205 rows and 15 columns. There are no inconsistencies or wrong data types, however there is a lot of missing data. ‘Description’ is missing in 142537 rows, price is missing in 141228 rows (almost 80% for both), ‘relative_results” are missing in 12589 rows and some other columns have missing values. Looking at the summary table, we can see that the average rating has a minimum of 1 and maximum of 5 which makes sense, but the median is 4.4 which is a little higher than on Yelp reviews which was
* The missing values for 'description' and 'price' are left and flagged.
* We checked if there are any duplicate reviews in the dataset and drop them. Rows affected: total - rows with no duplicated = 179205 - 178204 = 1001
* No wrong data types, or inconsistent formatting.
* We inspect other columns such as 'name' and 'state' to check if there is anything that doesn't make sense. Rows affected: none

4) The Google Local Reviews dataset has 100000 rows which we limited it to and 8 columns. There are few columns that have the wrong data type, user_id is stored as float and time is stored as a large integer such as(1.000000e+05). This dataset provides business info and customer reviews. Since there are some missing data such as ‘text’, ‘pics’, ‘resp’, we should assume that this is expected since some businesses just don’t have that available. Looking at summary statistics we can tell that time is written differently than expected, and ratings have a mean of 4.4 which is high.
* Missing values: Rows affected 29958
* We checked if there are any duplicate reviews from the same user at the same time in the dataset and drop them. Rows affected: 20204
* Wrong data types and formats change 'time' to correct format. Rows affected: 100000. For other wrong data types, we left and flagged them


## Section 3: Distributions of Important Features
This section goes through two important features in the Yelp business dataset which is one of more main primary datasets: restaurant star ratings and restaurant review counts. These features were selected because they describe customer satisfaction and customer engagement, which may be associated with restaurant closure status. Understanding their distributions helps identify typical values, spread, skewness, and potential outliers before examining relationships between features and restaurant closures.

### Distribution 1: Yelp Restaurant Star Ratings
Feature: stars
Plot: Distribution of Yelp Restaurant Star Ratings

Why this feature matters:

Restaurant star ratings provide a measure of customer satisfaction. Restaurants with lower ratings may face different challenges from highly rated restaurants, making ratings an important feature to investigate when examining whether customer satisfaction is associated with restaurant closure status. The histogram helps us understand the overall rating distribution and identify common rating ranges before comparing restaurants with different closure statuses.

Distribution shape and findings:
The analysis includes 52,268 Yelp restaurant businesses. The mean rating is 3.52 stars, the median is 3.50 stars, and the standard deviation is 0.83 stars. These results indicate that ratings are centered around 3.5 stars, with some variation across restaurants. The mean and median are very close, suggesting that the distribution is relatively balanced around its center. 

### Distribution 2: Restaurant Review Counts
Feature: review_count
Plot: Distribution of Yelp Restaurant Review Counts

Why this feature matters:

The number of reviews a restaurant receives measures customer engagement and provides a indicator of its visibility or popularity on Yelp. This feature is relevant because restaurants with different levels of customer engagement may also differ in their observed closure status. Review counts complement star ratings: ratings describe how customers evaluate a restaurant, while review counts describe how much feedback it has received.

Distribution shape and findings:
The histogram displays the distribution of review counts among Yelp restaurants.  Comparing the mean and median helps assess this pattern. If the mean is substantially greater than the median, this suggests that restaurants with high review counts pull the average upward.

## Section 4. Relationships:
### Relationship Plot 1: Review characteristics and restaurant closure status

This section explores the question: **How do average rating and review volume jointly relate to whether a Chicago restaurant is marked permanently closed?**

The unit of analysis in this plot is one restaurant. Review count is also used as an indicator for establishment popularity because the Google metadata provides a status at the time when the dataset snapshot was taken but no closure date. For this reason this is a descriptive association rather than evidence that reviews cause or predict an establisments closure.

### Preparation and status definition

Restaurants are identified when at least one Google category contains the word restaurant. Chicago records are identified from the city and state portion of the address. 

Duplicate gmap_id values are removed so that every business appears once.

For the outcome, **Permanently closed** is compared with nonmissing status strings that describe operating hours, such as "Open - Closes 10PM" or "Closed - Opens 11AM". Records marked **Temporarily closed** and records with missing status are excluded because they don't provide a defensible open vs. permanently-closed classification.

### Relationship: rating, review volume, and permanent closure

Review count is divided into quartiles so that each row of the heatmap has a substantial number of restaurants despite the strong right skew in review volume. Ratings are grouped into half-star-width bands. Each cell reports both the percentage marked permanently closed and the number of restaurants used to calculate that percentage. Very small cells should be interpreted cautiously.

#### Supplementary summary: review volume and permanent closure

The heatmap indicated nicely that review volume has the cleanest relationship with closure status. For this reason including a supplementary bar graph showing this relationship felt neccessary to make the overall pattern easier to compare. 

### 5. So what?

The clearest relationship is the review volume. Since we divided the review volumes into quartiles Q1, Q2, Q3, and Q4. Q1 were marked **52.0%** closed. Meaning that of the restaurants that had between 1-46 reviews on Google more than half had permenantly closed. Now compare that with only **7.0%** in Q4 (302-9,998 reviews). Moreover, restaurants with lower ratings had a closure rate. Overall: **45.9%** of restaurants rated below 3.0 out if 5.0 were marked permanently closed, compared with **22.3%** of restaurants rated 4.5-5.0. However, the rating pattern is less consistent within individual review-volume quartiles, and some low-rating cells have small sample sizes.

This relationship very important beacuse it suggest that review volume or amount is our proxy for popularity. And it may carry a stronger and more accurate relationship with operating status than average ratings alone.

This direction was expected because restaurants with more customer activity online should generally be more likely to keep their doors open. The size of the difference between Q1, lowest, and Q4, highest, review-volumes was more pronounced than we expected.

However, these results shouldn't be interpreted as a validated prediction on how an establishment will fare. Restraurants that remain open longer also have more time to accumlate reviews. Meaning, that businesses that have been established for a long time or exposure due to things like, franchises, may explain this pattern.

Google provides a snapshot status rather than a closure date, and businesses with missing or temporary status were excluded. A later predictive analysis would need time-indexed review features, a known outcome date, and controls for restaurant age, location, price, and category.

## Relationship Plot 2: Negative Google ratings and subsequent food inspection results

This section explores the question: **Do Chicago restaurants with worse food-inspection outcomes have a higher share of negative Google ratings beforehand?**

The second plot will compare primary Google Local Reviews data to the secondary Chicago Food Inspections dataset. To keep temporal order, review characteristics are calculated only from a year or 365 days before an inspection. One and two star ratings are classified as negative. Each restaurant contributes only its first qualifying inspection, and at least five years of ratings history are required.

### Restaurant linkage

This potential linkage error has to be mentioned. Since these two datasets dont share a common business ID, restraurants are therefore generated from neighboring latitude and longitude grid cells and found using exact geograpic data and business name and street number used as a backup ID. This is the reason for the extra code below.

A match is only accepted when either name similarity is at least 0.80 within 60 meters, or the street number agrees within 40 meters and the names have some moderate similarity. This allows for only the best candidate being retained for each inspection reducing false matches while allowing things like abbreviations.

### Constructing prior-review features

The Illinois review archive is streamed rather than loaded fully into memory. Only ratings and timestamps that can be linked to Chicago restaurants are retained. For every inspection, the negative reviews shared is calculated from ratings posted with in the 365 days. Datapoints with fewer than five ratings are removed. Also earliest inspection is selected for each restaurant so that businesses that get more inspections in the same amount of time due to their type (seafood restraunts) dont saturate the comparison.

### Relationship: prior negative ratings across inspection outcomes

The box plot compares the distribution of prior negative Google ratings and inspection outcomes.

### 5. So what?

With this box-plot were able observe that restaurants that failed an inspection had a slightly higher share of negative Google ratings during the preceding year. The failed group averaged **12.5%** negative ratings with a **9.1%** median, compared with **10.6%** and **7.9%** among restaurants that passed. Restaurants passing with conditions fell between them by mean **11.1%** and had a median of **7.9%**. All three distributions are right-skewed and overlap substantially.

This result was expected because poor restaurant cleanliness, food handling, and service condtions could affect both customer ratings and the afformentioned health inspections by Chicago authorities. However, the size of the differences were smaller than expected.

Failed inspections were only associated with only about 1.9 percentage points more negative ratins on average than a passing result. This suggests that ratings online may contain valid signals about an establisment's operational problems but are not strong enough to stand alone as an inspection to risk indicator. Written reviews mentioning these operational problems regaring cleanliness or safety maybe more accurate than star or numeric ratings alone.

This is an association, not evidence that negative reviews cause a direct inspection failiure. Things like restaurant type, location, price, review amount and inspection categories could affect both of these variables. Also linkage error could be very unlikely but possible beacuse Google and Chicago food-inspections lack a common identifier for each establisment, but since geograpic and name variables vere used as thresholds the linkage is error is very unlikely.


## Relationship Plot 3: Food-inspection results and later permanent closure

This section explores the question: **Are Chicago restaurants that fail a food inspection more likely to be marked permanently closed later, and does review volume change that pattern?**

This plot joins the two earlier analyses. Each restaurant's first qualifying inspection result (Plot 2) is linked by gmap_id to its Google status (Plot 1). Because the inspection happened while the restaurant was operating, the closure status comes later in time. Restaurants are also split at the median Google review count (188 reviews), since Plot 1 showed review volume is strongly related to closure. Only restaurants with a known "Observed operating" or "Permanently closed" status are included, which leaves 3,251 of the 4,838 restaurants from Plot 2.

### Relationship: inspection result, review volume, and permanent closure

The grouped bar chart shows the percentage of restaurants marked permanently closed for each first-inspection result (Pass, Pass w/ Conditions, Fail), split into restaurants at or below and above the median Google review count. Each bar reports its sample size.

### 5. So what?

Restaurants that failed their first inspection were marked permanently closed **16.4%** of the time, compared with **15.7%** of those that passed and **12.2%** of those that passed with conditions. That is a gap of only 0.7 percentage points between pass and fail, and the pass-with-conditions group had the lowest closure rate of the three. Review volume separates restaurants far more clearly. Restaurants at or below the median of 188 Google reviews closed more often than those above it in every inspection group (**25.0%** vs. **8.0%** for failures, 23.8% vs. 7.3% for passes, and 18.8% vs. 5.4% for passes with conditions).

This was somewhat unexpected. A failed inspection suggests operational problems, so a clearly higher closure rate seemed plausible, but the pass-fail difference was much smaller than expected. It is also far smaller than the review-volume gap from Plot 1 (52.0% vs. 7.0%). Together with Plot 2, where failed inspections were associated with only about 1.9 percentage points more negative ratings, this suggests that inspection outcomes are a weak signal of closure compared with review volume.

This is an association only. Google gives a snapshot status with no closure date, so we cannot say how long after the inspection a restaurant closed, and 2020 pandemic closures likely affect every group. A restaurant that was relocated or renamed may also be marked permanently closed. The sample has limits too. Of the 4,838 restaurants from Plot 2, only 3,251 have a known open or permanently closed status, and all of them had at least five Google ratings in the year before inspection. This leans toward more active restaurants, which is why the overall closure rate here is well below the 26.5% in Plot 1.

## 6. What next?

### Plot 1 (rating, review volume, and permanent closure)
**Next question:** Does review volume still predict closure once restaurant age is held constant?
Plot 1 showed a gap of 52.0% closed in Q1 vs. 7.0% in Q4, but older restaurants have had more time to collect reviews. The Chicago Business Licenses dataset (already loaded) has license dates that could give each restaurant an age and a rough closure date, though only 50,000 rows were pulled so coverage needs checking. I would also split by cuisine/category and by chain vs. independent, since chains may drive high review counts.

### Plot 2 (negative ratings before inspections)
**Next question:** Do the written reviews carry a signal that star ratings miss?
The failed group averaged only about 1.9 percentage points more negative ratings than the passed group (12.5% vs. 10.6%). I would pull review text from the same 365-day window and test for cleanliness and food-safety terms by inspection outcome. I would also split by inspection risk level, and check whether the five-rating minimum filtered the sample toward popular restaurants. A hand-check of a random sample of linked records would validate the matching.

### Plot 3 (inspection results and later closure)
**Next question:** Does inspection severity or timing explain closure better than a simple pass/fail label?
Failed restaurants closed only slightly more often than passed ones (16.4% vs. 15.7%), so pass/fail may be too coarse. I would look at the number and type of violations, the facility risk level, and repeat failures. I would also split by inspection year (2018-2019 vs. 2020-2021) to separate pandemic-era closures from earlier ones, and use license dates to estimate how long after an inspection closures happen. A national Yelp is_open check could test whether the review-volume pattern holds on another platform, though Yelp has only one Chicago business in this dataset.

### Research questions, restated

**Current questions:**
1. How do average rating and review volume relate to whether a Chicago restaurant is permanently closed?
2. Do Chicago restaurants with worse food-inspection outcomes have a higher share of negative Google ratings beforehand?

**Revised questions:**
1. Is review volume still associated with permanent closure after accounting for restaurant age, category, and chain status?
2. Do the content of written reviews, not just star ratings, signal worse inspection outcomes?
3. Do more detailed inspection measures (violation severity, risk level, repeat failures) help explain permanent closure beyond review volume?

**What changed and why:**
- **Question 1 now requires control for age.** Review volume was the strongest signal in Plots 1 and 3, but restaurant age is the most likely explanation, and the licenses data can address it.
- **Question 2 now includes review text.** The star-rating difference was small, so the written reviews are the better next signal.
- **Question 3 is new and narrower than expected.** Plot 3 showed that a simple pass/fail result barely relates to closure (16.4% vs. 15.7%), so the next step is to test whether finer-grained inspection details carry any signal that pass/fail hides.
