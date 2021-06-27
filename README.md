# Amazon_Vine_Analysis

## Overview
We were tasked with analyzing a dataset of Amazon reviews.  After leveraging PySpark to aid in the ETL process, we then had to determine if there was a positive bias in the reviews of paid Amazon Vine members.

## Results
In order to get the statistics that Jennifer was looking for, we first had to filter the dataset.  The first filter was to take out reviews with less than 20 votes:  `filter_total_votes_df = vine_df.filter("total_votes>20").select(["review_id", "star_rating", "helpful_votes", "total_votes", "vine", "verified_purchase"])`.  We then removed any reviews with a helpful vote to total vote ratio less than fifty percent: `filter_helpful_votes_df = filter_total_votes_df.filter("helpful_votes/total_votes>.5").select(["review_id", "star_rating", "helpful_votes", "total_votes", "vine", "verified_purchase"])`.  Finally, we created two new dataframes, one for vine members (`filter_Y_vine_df = filter_helpful_votes_df.filter("vine=='Y'").select(["review_id", "star_rating", "helpful_votes", "total_votes", "vine", "verified_purchase"])`) and one for non-vine members (`filter_N_vine_df = filter_helpful_votes_df.filter("vine=='N'").select(["review_id", "star_rating", "helpful_votes", "total_votes", "vine", "verified_purchase"])`)
From there, it was just a matter of filtering a little further and counting the rows in those filtered dataframes to arrive at the numbers we were asked to come up with:

```
from pyspark.sql.functions import count

paid_total = filter_Y_vine_df.count()
paid_5stars = filter_Y_vine_df.filter("star_rating==5").count()
paid_5stars_percentage = paid_5stars/paid_total
unpaid_total = filter_N_vine_df.count()
unpaid_5stars = filter_N_vine_df.filter("star_rating==5").count()
unpaid_5stars_percentage = unpaid_5stars/unpaid_total
```

- How many Vine reviews and non-Vine reviews were there?
  - Vine Reviews: 103
  - Non-Vine Reviews: 37372
- How many Vine reviews were 5 stars? How many non-Vine reviews were 5 stars?
  - 5 Star Vine Reviews: 55
  - 5 Star Non-Vine Reviews: 19723
- What percentage of Vine reviews were 5 stars? What percentage of non-Vine reviews were 5 stars?
  - 5 Star Vine Review Percentage: 53.4%
  - 5 Star Non-Vine Review Percentage: 52.8%

## Summary

