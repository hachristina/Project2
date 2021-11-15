# Board Game Data Set

The data set I have chosen for this project is from Kaggle (https://www.kaggle.com/andrewmvd/board-games?select=bgg_dataset.csv![image](https://user-images.githubusercontent.com/89666293/140661386-0f34eed2-0cf1-4c75-bb75-32a66b41b555.png). It is a data set with information collected from the Board Game Geek website’s database in February 2021. The data set contains all ranked games, which are games that have received at least 30 Board Game Geek, or BGG, users.

This puts the data set at 20,343 rows with 14 features for each row. Features include the game ID on the BGG website, name, year it was published, the minimum and maximum numbers of players suggested, the suggested minimum age for players, the number of users that have rated the game, the average rating received, BGG rank, average complexity, mechanics, and domain of the game. For this project, the target feature will be the rating average of a board game, making this a regression problem.

![image](https://user-images.githubusercontent.com/89666293/140661412-dd40063e-6f29-4612-9662-178e10b746c6.png)

After initial set up, the first tasks for this project were to clean the data. This included deleting unnecessary columns or duplicated rows. I decided to keep all the columns as I saw value in each of them. If I were to delete one, I might delete the BGG ID as that has likely little impact on the other features in this dataset. I decided to keep it though just in case. I checked for duplicated rows and didn’t see any. Also as part of this initial cleaning, I renamed the columns to make them more “Python”ic for example here I replaced the spaces between words with underscores. I also corrected inconsistencies and transformed datatypes. For example, the ratings used a comma instead of a period. So, I replaced the commas with periods and turned any object numbers like these into integers or floats.

Null Values: 
Also as part of cleaning, I identified and addressed missing values. Using the isnull.sum functions, I identified missing values in the ID, Name, Owned Users, Mechanics, and Domains columns.

![image](https://user-images.githubusercontent.com/89666293/140661441-ed86c4d5-4b3e-4424-9e79-3746bd1ba90f.png)

For the null IDs, I simply replaced the missing values with 0. My reasoning for this was that the ordinal value of the ID is not important and instead of deleting this small subset of the dataframe, I wanted to keep the data in the other features for these rows. For the missing Year Published value, I saw there was only one row that was missing this value so I decided to just drop the row as this is an extremely small subset of the data which contains over 20,000 rows. For the missing owned users values, I noticed a trend between the number of users rated and the number of owned users. After dividing the average value of users who owned the game by the average number of users who rated the game, I decided to use the assumption that the number of owned users is roughly 1.674 times more than the number of users rated. So, I used this average number and multiplied it by the number of users rated in the rows with missing owned users values to fill in those values. While there is definitely risk of these estimates being far off from the actual values, conceptually I think they are plausible enough and better to use than simply dropping the 23 rows where there are missing values. For the missing mechanics and domains values, I came to the same conclusion that there were too many rows with null values to just drop from the dataset. Instead, I replaced the null values with the string Undefined


After cleaning the data set, I began exploring the data. I created univariate visuals to examine distributions and any outliers. 
Beginning with the target feature,  Rating Average, I created a histogram and boxplot. These illustrations, along with the describe function, show that the mean rating for the dataset is 6.4 out of 10, with minimum and maximum ratings of 1.05 and 9.58 respectively. While the boxplot shows outliers, these are all acceptable because they are within the possible range 0 to 10

![image](https://user-images.githubusercontent.com/89666293/140661468-75fd1e4f-9ac2-460e-ab79-d10ee5f3b6bb.png)
![image](https://user-images.githubusercontent.com/89666293/140661473-0b139b6b-4725-46f2-9cb9-a0bff641ee7e.png)

I examined the other features in the dataset using histograms and boxplots. The boxplots were especially helpful in identifying outliers. Where appropriate, I dropped the rows that contained extreme outliers such as here with the boxplots for max players and play time. I looked at the top circle and determined that these outliers were extreme enough to disregard

![image](https://user-images.githubusercontent.com/89666293/140661484-6ef90d75-0ffa-43c5-a86d-63f7d7757c7f.png)
![image](https://user-images.githubusercontent.com/89666293/140661487-eeb04722-e6cf-4d57-910d-2f06d376a990.png)

In other cases with outliers however, I decided it wouldn’t be appropriate to simply remove the outlier rows. For example here, there were outliers in the number of user ratings. I decided to keep these outliers because I think that these games that have extreme number of reviews will also likely be games that are highly rated and may reveal trends that are important to people who want to create highly rated games.

![image](https://user-images.githubusercontent.com/89666293/140661498-b0137c69-e14f-4d5b-96a3-67abe7952862.png)

I created heatmaps to see correlations between the target feature and other numerical features in the dataset. I show four here that had the strongest relationships. Users rated and owned users seem to have a pretty clear positive relationship, complexity average seems to also have a positive relationship, though not as strong, and BGG rank seems to have a negative relationship where as the rank number increases, the rating average decreases.

![image](https://user-images.githubusercontent.com/89666293/140661506-4a7f155e-79fc-4d6b-b0f4-45f6178174d6.png)

Moving onto the categorical features, I examined the domains of the games in the dataset. I’ll describe more how I transformed the categorical features below, but for now, I’d like to show that the most common category was the Undefined category created from null values, and the next most common domains were war games and strategy games. 

![image](https://user-images.githubusercontent.com/89666293/140661568-625a0493-4a23-4819-b2a0-1f4c47d80998.png)

The other categorical feature to analyze was the mechanics feature.  Because there were 183 categories, the original barchart was overwhelming. Instead, I decided to just create a visual for the top 20 types of mechanics. Here we see that dice rolling and hand management games were the top most common mechanics.

![image](https://user-images.githubusercontent.com/89666293/140661580-20599f37-ad66-4e39-b1d4-5b746caad825.png)

The unique challenges in this data set came from having to manipulate the domain and mechanic features more than the others.  For both features, games could contain many values. For example here, the first game was listed as both a strategy game and a thematic games. The fourth game is listed as strategy and war game, and others being listed as three or more categories for these two features.  

![image](https://user-images.githubusercontent.com/89666293/140661598-2dac4a7a-aab5-4df5-b66b-aa1470ba08be.png)


To deal with this, I first had to turn the string in each column into lists by splitting the strings by commas and stripping the white space. Then, using help from a towards data science article, I created a function that turned returned a series, and then found the value counts of the individual types of domains and mechanics. This is what I used to create the bar charts for the most common Domains and Mechanics barcharts shown earlier above.

![image](https://user-images.githubusercontent.com/89666293/140661605-2036c862-f958-461f-b39c-26e4c616eb25.png)
![image](https://user-images.githubusercontent.com/89666293/140661642-d3af1fe7-5c43-488c-acf5-5f54e6c3eeab.png)

Next steps were to preprocess the data for modeling. I started by looking at the features and determining which ones were not numerical: Name, Mechanics, Domains. I dropped the Name column because this was not numerical and would not reveal trends across the data. Then, I used use MultiLabelBinarizer to turn the Mechanics and DOmains features into numerical values as this function works specifically for features that have list values.

I identified Rating Average as my target (X) vector and all other columns except X as my features matrix and then performed a train, test, split. 
I established a simple baseline "model" using the mean of the target to use later to compare to my model. The mean Rating Average was a 6.40, and the baseline model yielded high train and test set RMSE scores. 
Baseline Training RMSE: 0.9363136172084529
Baseline Testing RMSE: 0.9347815028684475

After establishing the baseline, I built a pipeline with Scaler, PCA, and KNN. After fitting to my training set, I printed the evaluation metrics: 

KNN Training RMSE: 0.571496151629592
KNN Testing RMSE: 0.7003582662754724

KNN Training R2: 0.627450537276609
KNN Testing R2: 0.4386663960088468

Because these scores weren't very accurate, I decided to also try a model using Linear Regression and Random Forest. 
These models yielded the following metrics: 

LinReg Training RMSE: 0.6522592177142857
LinReg Testing RMSE: 0.6825993951229261

LinReg Training R2: 0.5147139901189632
LinReg Testing R2: 0.46677276637478904

RF Training RMSE: 0.22302074912324923
RF Testing RMSE: 0.5977688973509652

RF Training R2: 0.9432654177473786
RF Testing R2: 0.5910717134751065

Because the RF R2 scores were higher than those of the KNN and Linear Regression models, I chose to hypertune the RF model to increase the R2 scores. 

I selected different ranges to test within the random forest hyperparameters: max depth, n estimators, and min samples split. The hypertuned RF model yielded the following metrics:
Tuned RF Training RMSE: 0.23146967953106967
Tuned RF Testing RMSE: 0.5943560121053459

Tuned RF Training R2: 0.9388853200706702
Tuned RF Testing R2: 0.595727831414443

The metrics are slightly better than the default model. This model explains roughly 60% of variance in the data and provides predictions that are roughly .59 off from the actual average game ratings. While these may not seem like ideal test scores, the model does yield a lower RSME score than the baseline model created using the target feature mean, which indicates that the Random Forest model provides more accurate predictions than if a model just predicted the dataset's mean rating for each game.

I attempted to identify which features are the most important in the model, but ran into errors. My next steps for this project would be to understand and correct the errors so that I could drop the least important features to improve the model as well as provide recommendations to game creators for which features to consider most to achieve high game ratings. 
