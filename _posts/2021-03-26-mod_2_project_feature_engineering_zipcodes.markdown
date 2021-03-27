---
layout: post
title:      "Mod 2 Project: Feature Engineering Zipcodes"
date:       2021-03-27 02:54:46 +0000
permalink:  mod_2_project_feature_engineering_zipcodes
---


Within King County House Sales dataset, 70 unique zipcodes were discovered. While it may be perfectly acceptable to one-hot encode this categorical variable, resulting in 69 rows (after dropping the first category), I wanted try out some feature engineering to reduce the amount of columns and make zipcodes more generalizable. 

## Goal:
Transform zipcodes into ranked system (1-10) based on median house sale price. 

## Process:
1) Group zipcode by median price

`zip_med_df = df.groupby(df['zipcode'])['price'].median().sort_values(ascending = False)`


2) Given 70 unique zipcodes and using a ranked system of 1-10, divide the zipcodes into 10 groups of 7, in which Rank 1 will contain the top 7 median house sale price zipcodes and Rank 10 will contain the bottom 7. House the rankings in a new column, titled "rank".

`zip_med_df['rank'] = np.divmod(np.arange(len(zip_med_df)),7)[0]+1`

<a href="https://imgur.com/gUfPO0J"><img src="https://i.imgur.com/gUfPO0J.png" title="source: imgur.com" /></a>


3) Merge the new rankings into the main dataframe.

`df = pd.merge(df, zip_med_df, on='zipcode')`

<a href="https://imgur.com/VjJII5z"><img src="https://i.imgur.com/VjJII5z.png" title="source: imgur.com" /></a>



