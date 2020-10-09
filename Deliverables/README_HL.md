# Helping a Car Collector Find the Right Spot

## Links to Relevant Notebooks

[Production Model](../Notebooks/production_model.ipynb)
[EDA and Cleaning](../Notebooks/eda_and_cleaning.ipynb)
[Kaggle Submissions](../Notebooks/making_submissions.ipynb)
[Ridge and Lasso](../Notebooks/ridge_lasso.ipynb)

## Problem Statement

I am a Realtor in the small town of Ames, Iowa. A potential client has come to you and asked you to give them a quote for the price their house in Ames would fetch if they put it up for sale. There is a twist: the client collects (and sells) cars, and therefore has a sizeable garage. They want advice for how best to upgrade their large garage before selling in order to fetch the highest price for their house. I am tasked with coming up with a quote of price predictions that can serve the client's interests - if I succeed, the client will book me and pay me a significant bonus. If not, the client will use my competitor, Mike Snakerson, who works across the street. That guy is a jerk.

## Data Description & Visualizations

We are using the Ames, Iowa Assessor's Office's data about houses sold in Ames between 2006 and 2010. This sample of the data includes information from 2,051 properties sold, with 81 distinct features.

The source data dictionary can be found [here](http://jse.amstat.org/v19n3/decock/DataDocumentation.txt). A paper by the dataset's original cleaner and the person who began the usage of the Ames, Iowa housing data can be found [here](http://jse.amstat.org/v19n3/decock.pdf).

The target of this particular project is the SalePrice column, which makes this a linear regression model.

Here is a data dictionary of the features we selected:

| Column Name   | Description                    | Type  |
| ------------- |:------------------------------:| -----:|
| Garage Qual   | Quality of the garage: Excellent, Good, Typical/Average, Fair, Poor  | object |
| Garage Cars   | Size of garage in car capacity |   float |
| Garage Area   | Size of garage in square feet  |   float |
| Garage Cond   | Condition of the garage: Excellent, Good, Typical/Average, Fair, Poor  | object |
| Garage Type   | Garage location: Two Types, Attached, Basement, Built-in, Carport, Detached  | object |
| Garage Finish  | Interior finish of garage: Finished, Rough-Finished, Unfinished  | object |


This correlation heatmap shows which columns out of the garage features I chose had the strongest correlation with the sale price of the houses:

[plot]: ./Images/garage_corr.png "Garage Correlations"

From looking at this correlation chart, we can deduce that Garage Area and Garage Cars have the strongest correlation overall to sale price - but the next strongest correlation, surprisingly, was Garage Finished - Unfinished, which was a negative correlation. Apparently, an unfinished garage could significantly reduce a home's sale price. With this information, we could explain to the client that if they are willing to finish their garage before selling it, they could significantly increase the price their home would fetch.

Here, we can show that houses with unfinished garages do not sell for the prices that houses with finished garages can fetch:

[plot]: ./Images/unfinished_price.png "Garage Finish vs. price"

In doing a closer examination of the Garage Area feature, we determined that the correlation was significant: a larger garage leads to a higher priced house, as can be seen from this plot:

[plot]: ./Images/area_price.png "Garage Area vs. Sale Price"

Finally, we multiplied the two columns with the strongest correlations - Garage Area and Garage Space - by each other to create a super column dedicated to the total space for cars inside a garage. Upon doing so, we plotted it against sale price, and were pleased to see an even stronger correlation:

[plot]: ./Images/space_price.png "Garage Space vs. Sale Price"

The conclusions we can draw from this are that our client will be selling for a higher price for their home with a garage spacious enough to host their car collection, but if they're unwilling to finish their garage, they may lose money.

## Evaluating Model Performance

When it came time to evaluate our model, we used a linear regression model and were disappointed to find low r2 scores and high root mean square error for our price predictions:

| Dataset        | Score Type   | Score  |
| ------------- |:-------------:| -----:|
| Training data | R2 Score | 0.61366 |
| Testing data  | R2 Score | 0.59807 |
| Both | RMSE | 49512.67 |

So, how could we update our model so it was better, but still closely reflect our client's particular home? Well, including some of the columns with the strongest correlations from the original dataset might help. So, we made the following chart:

[plot]: ./Images/numeric_corrs.png "Numeric columns vs. Sale Price"

Well, we can see that Overall Quality of the home and the Above Ground Living Area (in sq. ft.) are very strongly correlated with increasing sale price. Additionally, the Total Basement Square Footage, 1st Floor Square Footage, Year Built, and Year Remodeled or Added Onto also strongly correlated to an increase in price. So, we included those columns - and an interaction column that multiplied their correlation - in our model to increase its functionality. Following their inclusion, the model significantly improved:

| Dataset        | Score Type   | Score  |
| ------------- |:-------------:| -----:|
| Training data | R2 Score | 0.87886 |
| Testing data  | R2 Score | 0.87967 |
| Both | RMSE | 27531.20 |

So, we significantly increased our R2 scores, and decreased our root mean square error by more than 20,000. While adhering to our client's specifications, we believe this is the most functional model we can produce.

## Primary Findings/Conclusions

We have created a highly-functioning model for our client, and hope we can use it to help them sell their home for the highest possible price. Here, we can see a plot of our predictions vs. the recorded sale prices, closely following the slope of the sale price line:

[plot]: preds.png "Predictions vs. Reality"

We have determined that, in looking for the highest potential price for the client's specific home, if they want a high selling price, they should finish their garage and advertise the spacious garage that they have. Additionally, if they want to increase the home's price even more, boasting about the home's quality and total square footage will boost the price even more.

## Next Steps

Refining and improving the model for the client would involve getting more information about the garages of the homes that have sold - for example what are the costs of upgrading a garage? Would the higher price the house could fetch offset the price of upgrading to a finished garage? Essentially, next steps would involve delving deeper and researching the client's specific needs.

Improving the model might also involve including more house features, but adhering to the client's specifications and molding the model to them was our utmost concern. Additionally, to tailor even more specifically to the client's needs, it might make sense to get more information about the exact size of the garage and print a quote of prices that 

