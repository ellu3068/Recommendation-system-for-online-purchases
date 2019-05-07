# Recommendation-system-for-online-purchases
In this project, Elasticsearch was used to build a recommendation system for an online retail company.

The inspiration is drawn from this article (building something similar for ingredients in recipes): [A recommender system for ingredients in recipes](https://qbox.io/blog/building-simple-recommender-systems-for-elasticsearch-1) 


## The dataset
The dataset consists of items purchased from an online retailer over the course of a year or two.
The dataset contains x observations and eights columns: InvoiceNo, StockCode, Description, Quantity, InvoiceDate, UnitPrice, CustomerID, and Country. 
To better prepare the dataset for elasticsearch and queries, serveral cleanning steps were implemented:
1. All rows cotain NAs were dropped.
2. Special symbols, including quotation mark ("), question mark (?), and period (.) were removed.
3. Accodring to the documentation, ff InvoiceNo starts with letter 'C', it indicates a cancellation. To avoid unnecessary complications, all rows with InvoiceNo started with a "C" is remove.

You will use Elasticsearch to
learn what items are frequently purchased together and use this information to make future recommendations
(much like Amazon's "Customers who bought this also bought <whatever>").  This is called an "item-item"
recommender.

Here is the dataset:

[Retail dataset](https://archive.ics.uci.edu/ml/datasets/online+retail#)

It is good practice to split your dataset into training and test sets.  Since they are
time-ordered, perhaps choose the first 80% of invoices as training, and the rest as a test set.
Use the test set to validate your recommendations.  Do they make sense?
