# Amazon_Vine_Analysis

## Overview
We were tasked with analyzing a dataset of Amazon reviews.  After leveraging PySpark to aid in the ETL process, we then had to determine if there was a positive bias in the reviews of paid Amazon Vine members.

## Results
In order to get the statistics that Jennifer was looking for, we first had to filter the dataset.  The first filter was to take out reviews with less than 20 votes:  `filter_total_votes_df = vine_df.filter("total_votes>20").select(["review_id", "star_rating", "helpful_votes", "total_votes", "vine", "verified_purchase"])`.  We then removed any reviews with a helpful vote to total vote ratio less than fifty percent: `filter_helpful_votes_df = filter_total_votes_df.filter("helpful_votes/total_votes>.5").select(["review_id", "star_rating", "helpful_votes", "total_votes", "vine", "verified_purchase"])`.  Finally, we created two new dataframes, one for vine members (`filter_Y_vine_df = filter_helpful_votes_df.filter("vine=='Y'").select(["review_id", "star_rating", "helpful_votes", "total_votes", "vine", "verified_purchase"])`) and one for non-vine members (`filter_N_vine_df = filter_helpful_votes_df.filter("vine=='N'").select(["review_id", "star_rating", "helpful_votes", "total_votes", "vine", "verified_purchase"])`)
