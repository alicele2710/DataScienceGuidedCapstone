# Pricing and Competitor Analysis for Big Mountain Resort 

The purpose of this data science project is to come up with a pricing model for ski resort tickets in our market segment. Big Mountain suspects it may not be maximizing its returns, relative to its position in the market. It also does not have a strong sense of what facilities matter most to visitors, particularly which ones they're most likely to pay more for. This project aims to build a predictive model for ticket price based on a number of facilities, or properties, boasted by resorts (at the resorts). This model will be used to provide guidance for Big Mountain's pricing and future facility investment plans.

## 1. Data Cleaning
The orignal dataset contains 330 entries, including our own resort. Column fastEight was removed because it had more than 50% missing data, the rest were 0. We also dropped rows missing both ticket price datas.

SkiableTerrain_ac and Snow Making_ac distributions are clustered at the lower end. It turns out that there were outliers. As we cross-checked with another data source, we corrected these outliers and the distributions look more plausible. Another problem is that the yearOpen column has maximun value = 2019, which is likely the year when the resort was open, not the period it has been operation.

We can predict pricing using either AdultWeekday or AdultWeekend. As AdultWeekend has fewer missing values than AdultWeekday, we dropped AdultWeekday column and all rows missing AdultWeekend data. We have 277 rows left after cleaning.<br>
<img width="256" alt="image" src="https://user-images.githubusercontent.com/70331438/224581828-a073140f-f787-4533-9624-bf1c82f4b526.png">
<br>
## 2. Exploratory Data Analysis
[Feature correlation heatmap]<br>
<img width="674" alt="image" src="https://user-images.githubusercontent.com/70331438/224582741-7f0f4e3b-425f-474b-8f7e-23ac7dd03602.png"><br>
There is no strong pattern that suggest a relationship between state and ticket price. Therefore, we decided to focus on resort level data (including each resort's share of state-wise supply). We should be careful of multicolinearity among the features (number of resorts per state and each resort's share of supply metrics). To select target features, we look for those with positive correlation to AdultWeekend. State labels may affect the ticket price as some states are more popular for skiing, hence command higher ticket prices regardless of the resorts' assets. We need to retain the state labels or check with business team to confirm which states are highly popular for skiing.

## 3. Pre-Processing and Training Data 


We have gained a baseline idea of performance by taking the average price (MAE $19 on test set).

We built a linear model which works better on the train set than on the test set. We suspected that the model was overfitting.

We then used grid search to find the optimal number of features. Vertical drop was the biggest positive feature, and Tram/SkiableTerrain_ac were the biggest negative features. The estimate of its performance from cross-validation was 10.5 with 1.6 standard deviation. The test split was consistent with this estimate (MAE = 11.8)<br>
<img width="419" alt="image" src="https://user-images.githubusercontent.com/70331438/224583561-9a231247-d244-4f78-9e72-ec6b401d6f5a.png"><br>

We also built a random forest model and tried out different number of trees, imputing strategies, and doing/not doing feature scaling. The estimate of its performance from cross-validation was 9.6 with 1.4 standard deviation. The test split was consistent with this estimate (MAE = 9.5)
<br><img width="405" alt="image" src="https://user-images.githubusercontent.com/70331438/224583624-98563975-0311-4071-a64e-23955a18daa1.png"><br>
We decided to go ahead with the 2nd model which had a better performance overall (smaller MEA and standard deviation) and performed better on the test set.

## 4. Modeling
### 4.1. Modeling results
Big Mountain Resort modeled price is 95.87, actual price is 81.00. Even with the expected mean absolute error of 10.39, this suggests there is room for an increase.

This is justified from business standpoint as the resort ranks high in all important facilities (vertical drop, snow making area, run, etc.)

To cover the operating cost 1.5M from the additional chair lift this season, Big Mountain has to increase 0.88/ticket.

We have modeled 4 different scenarios and the one that has strongest impact on ticket price (+1.99 ticket price, + 3.5M revenue) is adding 1 run + increase vertical drop by 150 ft + adding 1 chair lift (scenario 2).

Scenario 1 suggested that removing 1 run doesn't negatively impact the ticket price. We can further test if removing 1 run (instead of adding 1 run) doesn't impact the result in scenario 2.

### 4.2. Deficiencies in the data that hampered or limited this work
When customers consider skiing resorts, they perhaps look at the total costs of the trip (including travel, lodging, food, etc.). Having these info across all resorts may be helpful for us to price our ticket optimally.

The modeled price may not come as a surprise for business executives since Big Mountain was already fairly high on some of the league charts of facilities offered. However, we don't know how has the "discounted" price been driving up the number of visitors for Big Mountain. Only if the total costs (not only the ticket price) incurred by customers visiting Big Mountain are competitive, then we can guarantee the increase in revenue.

How can the business make use of the model: Deploy it into tool that allows business teams to make adjustments to different features and see the predicted impact on ticket price and revenue.
