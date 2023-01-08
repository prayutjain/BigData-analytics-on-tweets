# BigData analytics on tweets - use of MinHash to test similarity of texts

Our objective is to analyze the tweets concerning topics of education in 2022 and figure out the trends and other visible dynamics in the education sector. This analysis was performed in pySpark on Google Clous clusters for the tweets related to topics of education, schools, universities, learning, knowledge sharing, etc. The aim of this exercise is to highlight the following:

1. Who are the influencers in the education sector?
2. Are these influencers also trendsetters in the education sector?
3. Is Twitter reflective of issues/trends in this sector which can be further used to improve the existing education framework?
4. Are the tweets original or just copies of other tweets

------------------------------------------------------------------------------------------------------------------------

## Data

The total size of the data was ~500 GB, spread over multiple .json files which was obtained from twitter API. The data contains tweets from users during the period April Nov 2022 and had around 100 million tweets. The data is structured to have multiple columns of information corresponding to each tweet, such as the user who tweeted and its details, the likes/comments/retweets the tweet got, the location from which the tweets was made, and other entities ( https://dev.twitter.com/overview/api/tweets )

-------------------------------------------------------------------------------------------------------------------------

## Methodology

I will be using PySpark for the entire data analysis and various python libraries such as pandas, matplotlib for data visualisation. For analysing the similarities, I used functionalities such as MinHash and LSH using Jaccard distance metric.

### Data cleaning

I carried out a simple filtering process to select only the tweets which had certain keywords related to the topics of education and discard the tweets which might not be directly related to trends in education sector. For this, I used a website http://relatedwords.com to search for all related words.

* Also removed the tweets which were sensitive and had offensive keywords
* Finally, I am using only ~30 million tweets for our analysis
* I also observed that ~60% of the tweets are retweets and separated them for most part of the analysis, except those otherwise mentioned

For tweet similarity analysis, we did the following data cleaning:
* Removed non alphanumeric words/text and lowered the case
* Removed stopWords from english dictionary

### For feature selection

* After a close look at the data, a lot of columns were very poorly populated and had to be discarded. I ran a coverage check on the 30M tweets and kept only the column which had more than 70% non missing data for our further analysis
* I also observed a lot of nested data structures, which would create difficulties in the analysis. So I flattened the data for further use.

### To identify the influential twitterers (if any)

I created an influence score to check the popularity and relevance of their tweets
* Using a simple metric, such as volume of tweets and volume of retweets
* Also by creating a comparative metric
    * Popularity score = Avg count of likes on tweets / Total no. of followers
    * Relevance score = Avg count of reply, quote, and retweet) / Sum( Avg count of reply, quote, retweet and likes)

-------------------------------------------------------------------------------------------------------------------------

## Conclusions

1. After discarding irrelevant tweets, we are left with ~30 million tweets and most of them are unverified. News organizations have the highest volume of tweets (both in general and by influencer Twitter handles) which is a credible source of information.
2. The same fact is observed when we ran the analysis on top 100 influencers, and the most tweets are from news organizations
3. The highlighted influencers are from varied backgrounds and have high follower counts too. We also observe the popularity and relevance scores for these influencers which seem significant
4. Most of the twitterers and tweets are from the North American region which could be because of the tweets regarding the Uvalde shooting.
5. Looking at the timeline, there are two peaks in no of tweets observed in May and Aug Sept corresponding to the Uvalde shooting and thestart of the new term/application process.
6. From the uniqueness analysis, most of the tweets are unique. But this analysis was based on a small dataset and the conclusion might vary with the entire dataset, though it requires higher computation power too.

-------------------------------------------------------------------------------------------------------------------------

## Actionable recommendations

1. With the present level of accuracy in analysis, it is risky to say that this data could be a credible source of information. But this definitely had the information we are looking for. With the right set of tools, maybe a meaningful model can be developed.
2. Working with such noisy data requires caution. In such cases, minimizing the assumptions should be the priority and the following points should be kept in mind - Twitter is a platform where people put their opinion and trusting any random opinion is difficult. So devising a mechanism to ch eck the
credibility of a source is important before considering it as the marker of the emergence of new trends
3. Sentiment analysis would be a good step to understand the tone of each tweet and might give information about the general mood of the public about a particular trend.
4. Data cleaning could be the biggest hiccup where methods like k means clustering etc. should be considered to select a cluster of tweets instead of word based filtering
5. Retweet/message volume is not the best indicator of the relevance/popularity of a particular tweet. There could be better score could be generated to rank the influencers.
5.A study more specific to each region would say more than a general one. Because a lot of topics in education would have only a local impact.
