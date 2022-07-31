# WeRateDogs_Tweets_Wrangling_and_Analysis
The entire data wrangling process as well as analysis of the WeRateDogs Tweets.

WeRateDogs is a Twitter account that rates people's dogs with a humorous comment about the dog. These ratings almost always have a denominator of 10. The numerators, though? Almost always greater than 10. 11/10, 12/10, 13/10, etc. Why? Because "[they're good dogs Brent](http://knowyourmeme.com/memes/theyre-good-dogs-brent)." WeRateDogs has over 4 million followers and has received international media coverage.


# Datasets Description
In this project, I worked on the following three datasets.

## Enhanced Twitter Archive
The WeRateDogs Twitter archive contains basic tweet data (tweet ID, timestamp, text, etc.) for all 5000+ of their tweets as they stood on August 1, 2017, but not everything. One column the archive does contain though: each tweet's text, which was used to extract rating, dog name, and dog "stage" (i.e. doggo, floofer, pupper, and puppo) to make this Twitter archive "enhanced." 

## Image Predictions File
This is a table(downloaded programmatically) full of image predictions (the top three only) alongside each tweet ID, image URL, and the image number that corresponded to the most confident prediction (numbered 1 to 4 since tweets can have up to four images).

## Tweet Json File
Back to the basic-ness of Twitter archives: retweet count and favorite count are two of the notable column omissions. These two columns and more can be found in the tweet json file.


# Wrangling Procedures
The following are the steps taken to wrangle these datasets.

## Data Gathering
I gathered data from three different sources. The procedure is as follows:
- I directly downloaded the WeRateDogs Twitter archive data (twitter_archive_enhanced.csv)and then imported it as a dataframe.
- I then used the Requests library to download the tweet image prediction(image_predictions.tsv) and imported it as a dataframe as well.
- I downloaded the tweet-json.txt file and then using json.loads() I retrieved the tweet_id, retweet_count and favorite_count. Then I converted the extracted data into a dataframe.

## Assessing Data
I assessed all three datasets both visually and programmatically to identify both quality and tidyness issues. The issues I identified are highlighted below:

### Quality issues
- Some of the records in the twitter_archive dataframe are retweets.
- Some of the records in the twitter_archive dataframe are replies.
- Invalid datatype for the tweet_id in all three dataframes and the timestamp column in thetwitter_archive dataframe.
 -Some of the tweets in the twitter_archive dataframe are far beyond August 1st, 2017.
- In the twitter_archive dataframe, some values in the rating_denominator are not 10.
- In the twitter_archive dataframe, some values in the name column are 'a', 'an', 'one' 'this' where itshould be 'None'.
- In the twitter_archive dataframe, the columns rating_numerator and rating_denominator shouldbe converted to one column ratings defining the rating ratio to effectively describe each dogsratings and for analysis.
- There are columns that are irrelevant to analysis in the twitter_archive dataframe and images_df dataframe. They are in_reply_to_status_id, in_reply_to_user_id, source, retweeted_status_id,retweeted_status_user_id, retweeted_status_timestamp, expanded_urls for the twitter_archivedataframe and p1_conf, p1_dog, p2, p2_conf, p2_dog, p3, p3_conf, p3_dog for the images_dfdataframe. Column p1 in images_clean is not a descriptive column name.
### Tidiness issues
- In the twitter_archive dataframe, the columns doggo, floofer, pupper, and puppo should be one column as they all specify dog status.
- The three dataframes should be one dataframe containing the tweet_ids and the corresponding attributes of each dog tweet which is present in all three dataframes.

## Cleaning Data
I cleaned all of the issues I had documented while assessing the data. I also made sure to make a copy of the original data before cleaning. The result of the cleaning was a tidy dataset.

## Storing Data
I saved the final cleaned dataset as a csv file using the .to_csv() function.

## Analyzing and Visualizing Data
After the data wrangling process, I proceeded to analyse the final cleaned dataset. The following questions were asked and analysed for this dataset: 
- What dog status has the highest rating_ratio on average? 
Act: For this analysis question, I grouped the dataframe by the dog_status column using the.groupby() method and then calculated the average rating_ratio using the mean() function. 
Result: The dog_status 'doggo, puppo' has the highest average rating_ratio of 1.300000.
![Average rating_ratio by dog_status](https://user-images.githubusercontent.com/64794688/182009512-99ab795a-00ab-4a85-a40e-5b65ec841643.png)

- What dog status has the highest favorite_count on average? 
Act: For this analysis question, I grouped the dataframe by the dog_status column using the.groupby() method and then calculated the average favorite_count using the mean() function. 
Result: The dog_status 'doggo, puppo' has the highest average favorite_count of 47844.000000.
![Average favorite_count by dog_status](https://user-images.githubusercontent.com/64794688/182009537-3a07c465-e3d8-4ddc-8c61-5ec2d79c59b9.png)

- What type of relationship or correlation exists between rating_ratio and favorite_count?
Act: For this analysis question, I first did a scatterplot to get an insight into what kind of correlation exists between rating_ratio and favorite_count. Then, I did a boxplot to visualize the distribution ofthe rating_ratio column and found that an outlier exists in the data. Then, using the .query()method, I queried the the rating_ratio column to seperate into high and low rating_ratio using the median which I got from the result of the .describe() function on the rating_ratio column. Finally, I calculated the average favorite_count using the mean() function.
Result:
High rated dogs average favorite_count: 15977.864973262032 and 
Low rated dogs average favorite_count: 4597.8378378378375. 
Highly rated dogs have a significantly lower favorite_count than low rated dogs. The fact that a dog is highly rated doesn't imply that it gets higher favorite_count.

- What type of relationship or correlation exists between rating_ratio and retweet_count?
Act: Using the high and low rating_ratio gotten beforehand, I calculated the average favorite_countusing the mean() function.
Result:
High rated dogs average retweet_count: 4737.729946524064 and 
Low rated dogs average retweet_count: 1579.8206388206388. 
Highly rated dogs have a significantly higher retweet_count than low rated dogs.
