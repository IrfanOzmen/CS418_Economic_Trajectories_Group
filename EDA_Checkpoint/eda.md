

##1.
##2.

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


##6.