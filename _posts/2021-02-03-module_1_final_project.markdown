---
layout: post
title:      "Module 1 Final Project"
date:       2021-02-04 02:41:46 +0000
permalink:  module_1_final_project
---

For my first ever data science project, I was tasked to provide actionable insights and recommendations for Microsoft's newly created movie studio (hypothetically).

### Step 1: Explore the data
Off the bat, I was overwhelmed with the 11 data sets provided and did not know where to start. After countless hours exploring the different data sets, I finally came up with the idea of structuring my analysis around budgets and profits. This turned out to work in my favor as I was able to narrow it down to 3 fairly clean data sets. 

### Step 2: Clean the data
I decided to clean each data set individually prior to merging them into a single dataframe. This process included removing $ signs, commas, duplicate columns, standarding column names, converting data types, etc. Additionally, as an extra precaution, I used the lower() method to convert all uppercase characters in the title column into lowercase characters to prevent any case issues when merging.
```
rating_df['title'] = rating_df['title'].str.lower()
```
Once each data set was cleaned, I merged the 3 data sets into 1 dataframe based on the title. Here comes data cleaning part 2. While going through the data cleaning process, one column caught my eye. I noticed that a lot of movies had multiple genres and variations of it (i.e. justice league	= action,adventure,fantasy and avengers: infinity war = action,adventure,sci-fi). Instead of breaking it down into unique genres, I decided to lump them all under cross-genre. I used the comma as the identifer for cross-genre movies. 
```
final_df.loc[final_df['genres'].str.contains(','), 'genres'] = 'cross-genre'
```
Side note: This did not work in my favor. 83% (1551 of 1879) of the movies were cross-genre. I realzied that I did not have enough enough data points to make a fair recommendation for unique genres and dropped further exploration.

### Step 3: Visualize the data
I will highlight 2 of my exploratory questions:

#### 1. How much should Microsoft budget to make a highly-profitable movie?
Using matplotlib, I created a scatter plot and added a trend line.

```
font = {'family': 'Times New Roman',
        'color':  'black',
        'weight': 'normal',
        'size': 16,
        }

plt.style.use('ggplot')

fig, ax = plt.subplots(figsize = (16, 8))
x = final_df['production_budget']/1000000
y = final_df['profit']/1000000
ax.plot(x, y, 'o')

m, b = np.polyfit(x, y, 1)

ax.plot(x, m*x+b)
plt.xlabel('Budget (millions $)', fontdict=font)
plt.ylabel('Profit (millions $)', fontdict=font)
plt.title('Budget vs Profit', fontdict=font)

plt.show()
```
<a href="https://imgur.com/Qp8mcEX"><img src="https://i.imgur.com/Qp8mcEX.png" title="source: imgur.com" /></a>
The correlation between budget and profit was moderately high at 0.69. Based on the scatter plot, I noticed a breakway point for movies with a budget of at least $200 million where profits were trending up. I recommend that Microsoft consider budgeting that amount at a minimum. 

#### 2. How much should Microsoft budget to make a highly-profitable movie?
Using matplotlib, I created a bar plot by release month and average profit with an additional line displaying the average profit of all movies in my data set. 
```
fig, ax = plt.subplots(figsize=(16,8))

x = mean_month_profit_df['release_month']
y = mean_month_profit_df['profit']/1000000

ax.bar(x,y)

ax.set_xlabel('Release Month', fontdict=font)
ax.set_ylabel('AVG Profit (millions $)', fontdict=font)
ax.set_title('Release Month vs AVG Profit', fontdict=font)
ax.set_xticks(x)

plt.axhline(mean_month_profit_df['profit'].mean()/1000000, color='black', label='Overall AVG Profit');
plt.legend()
plt.show()
```
<a href="https://imgur.com/2wfDN1p"><img src="https://i.imgur.com/2wfDN1p.png?1" title="source: imgur.com" /></a>
Most profitable movies on average are released in May-July and November-December. I recommend Microsoft consider releasing movies in June to expect the highest profits. 

### My key takeaways
This was a fun first project to get my feet wet in data science. But if I were to redo this project, I would like to explore unique genres in detail, instead of categorizing movies with multiple genres as cross-genre. A method that could help me with this exploration is explode().

