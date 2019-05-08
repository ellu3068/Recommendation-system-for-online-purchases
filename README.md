# Recommendation-system-for-online-purchases
In this project, Elasticsearch was used to build a recommendation system for an online retail company.

The inspiration is drawn from this article (building something similar for ingredients in recipes): [A recommender system for ingredients in recipes](https://qbox.io/blog/building-simple-recommender-systems-for-elasticsearch-1) 

There are two goals for the project:
1. Utilized Elasticsearch to build recommendation system.
2. Compare User-Item Recommender to Item-Item Recommender. The hypothesis is that the User-Item Recommender would outperform the Item-Item Recommender.

## The Dataset
The dataset consists of items purchased from an online retailer over the course of a year or two.
The dataset contains x observations and eights columns: InvoiceNo, StockCode, Description, Quantity, InvoiceDate, UnitPrice, CustomerID, and Country. 

Here is the dataset: [Retail dataset](https://archive.ics.uci.edu/ml/datasets/online+retail#)

To better prepare the dataset for elasticsearch and queries, serveral cleanning steps were implemented:
1. All rows cotain NAs were dropped.
2. Special symbols, including quotation mark ("), question mark (?), and period (.) were removed.
3. Accodring to the documentation, ff InvoiceNo starts with letter 'C', it indicates a cancellation. To avoid unnecessary complications, all rows with InvoiceNo started with a "C" is remove.

The dataframe was then prep differently for User-Item Recommender and Item-Item Recommender.
For Item-Item Recommender, the data was aaggregated by InvoiceNo. The data was then ordered by InvoiceDate before splitting into first 80% tranining and later 20% testing. We later refer to this data as _item_data_.
For User-Item Recommender, the data was aaggregated by CustomerID. A new column was created based on the frequency (by quartile) of visit of the customer. The data was then randomly split into 80% tranining and 20% testing. We later refer to this data as _cust_data_.

All data was fling into kibana.

## The Model
3 models were constructed:
1. Item-Item Recommender: The query aggregated the training dataset of _item_data_, where significant terms were found based on the input terms from the testing set of _item_data_. For example, given an InvoiceNo from testing set has three items being purhcased: A, B, and C. The recommender made prediction for what items B and C by finding the two most correlated items with A based on the training set. Given the prediction is B and C, the accuracy is calculated by dividing the overlap item(s) by the total predicting items. In this case, the prediction accuracy is 1/2 = 50%. After repeating this same procedures for item B and C, the accuracy of an InvoiceNo is calculated by avergaing the prediction accuracy of all items. The final accuracy of the Item-Item Recommender is calculated by taking the mean accuracy of all InvoiceNo.
2. First User-Item Recommender: The query aggregated the training dataset of _cust_data_, where significant terms were found based on the input terms from the testing set of _cust_data_. The same query and calculation were conducted, except the prediction was made for each CustomerID instead of InvoiceNo.
3. Second User-Item Recommender: This recommender was based on the same exact data and procedure as the first User-Item Recommender. However, the query was more selective where the query not only filtered by the item, but also by the visit frequency of the customer. In the assumption is that, by doing so, we can narrow down to the same customer group with similar purhase behavior, thus increase the predicting accuracy. 

## The Result and Analysis
#### Prediction 
Accuracy | Item-Item Recommender | First User-Item Recommender | Second User-Item Recommender
 | ------------ | ------------- | ------------ | -------------
Min | 0% | 0% | 0%
Max | 75% | 56.6% | 
Average | 8.2% | |  
