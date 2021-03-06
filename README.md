# Goodreads Book Recommendation System
*Outside of visual mediums, such as television or movies, books are one of the highest forms of entertainment. They can be informative with historical exploits, as well as delightful diversions from reality, and everything in-between. It is because of this last fact that booksellers such as Barnes & Nobles or Amazon can offer such wide varieties of books, like casting a wide net, that appeal to a large customer base to visit and purchase their wares. However, from the customer's perspective, choosing a book to read next can be difficult.*

*Booksellers and online retailers often use Co-oping as a strategy in order to advertise new works or books that have recently been released. This means publishing companies personally ask bookstores for preferential treatment of their books or series. However, when it comes to tailoring to individual users, this kind of advertising is not very effective. Given the fact that online retailers can see a user’s previous purchasing history and maintain a better record of it than brick-and-mortar bookstores, there now exists the option to tailor recommendations specifically to individuals. This type of system could also be used in other websites such as Goodreads.com.*

*Within this project, using a dataset from Goodreads.com, we intend to create a content-based recommendation system that can be used to recommend books to different users. This would also make a good base system for a future collaborative recommendation system that would use user data in addition to further improve predictive accuracy.*

## 1. Data
The dataset was downloaded from Kaggle.com, from a CSV file that was created using Goodreads.com’s API. The dataset contains a list of book titles along with those books’ authors, isbn numbers, average rating of a book, total number of ratings given, and total number of text reviews. There are over 13714 different entries for this dataset, with a few book titles that repeated themselves. The link to the website and the location of the dataset can be found below:

>* [Goodreads Dataset from Kaggle.com](https://www.kaggle.com/jealousleopard/goodreadsbooks)

## 2. Data Cleaning
>* [Data Cleaning Notebook](https://github.com/R-Yamin/Book_Recommendation_System/blob/master/1.%20Data_Cleaning.ipynb)

To begin, we looked over the dataset to see if there were any outstanding issues that might make further analysis difficult. Fortunately, there was no need for much cleaning since the dataset was relatively neat. There were only a few minor corrections that were done for clarity in order to tidy up the data further.
### 2a. Misaligned columns:
When trying to first read the CSV file into the Jupyter Notebook, it produced an error. It was then discovered that there were a few rows that were problematic. These rows did not line up well with the rest of the dataframe because they contained an extra column within each row. It was decided to skip these rows, since it was only 6 points in the data.
### 2b. Multiple Authors:
Looking at the authors column, it became clear that some books had more than one name attached to them. While some of these names were actually the illustrators for the books (such as with the case of Harry Potter), others were translators, voice actors for audiobooks, or actually co-authors of the book itself. The book Good Omens was written both by Terry Pratchet and Neil Gaiman, and so both could be attributed as the authors.
Since it was unclear to what extent the use of authors could be in the future analysis, it was decided to split the authors’ column into two separate columns: the primary author and the secondary author. The first author in the column was designated the primary author since they were more often attributed to the book in that row, and the secondary author was whoever remained. This would make any comparison by authors in the future more accurate since readers may be more interested in the primary author rather than the secondary.

>* [Cleaned Dataset](https://github.com/R-Yamin/Book_Recommendation_System/tree/master/Clean_data)

## 3. EDA
>* [EDA Notebook](https://github.com/R-Yamin/Book_Recommendation_System/blob/master/2.%20EDA%20of%20Goodread%20Books.ipynb)

Certain columns of the data were unnecessary to explore because they would not typically be considered when searching for a book. These columns were the bookID and the isbn columns. Outside of those columns, the data was explored in depth and the full analysis can be found in the notebook link above. Below are the analyses of significance.

### 3a. Most Frequent Book Titles
Since there were over 13700 book titles, we wanted to check if any repeated. As it turns out, there were 1292 repeated titles. Below is a snippet of the full a horizontal bar graph listing the most frequent titles and how many times they repeated.
![](https://github.com/R-Yamin/Book_Recommendation_System/blob/master/Saved_images/book_title_frequency.png)
The most repeats for a title was 11 repeats, which is not too bad for the dataset. Furthermore, since these repeats were for potential different versions of a book, and since there were only 1292 out of 13 thousand samples, the titles were not removed. It did mean that for the recommendation system that there should be a second way to specify which book.

### 3b. Distribution of Average Ratings
Below is a graph for the distribution of average ratings. Shown in solid red is the mean and in dashed red lines is the standard deviation. Most of the distribution fell pretty much between 3.6 and 4.2 for average rating.
![](https://github.com/R-Yamin/Book_Recommendation_System/blob/master/Saved_images/Ratings_distribution.png)
>* [Statistical Analysis Notebook](https://github.com/R-Yamin/Book_Recommendation_System/blob/master/3.%20Statistical%20Analysis.ipynb)

### 3c. Number of Pages Vs. Ratings
Below is the graphs for number of pages over average rating and number of pages over total number of reviews:

![](https://github.com/R-Yamin/Book_Recommendation_System/blob/master/Saved_images/Ratings_pages_reviews.png)

Initially, looking at the graph to the left with number of pages over average rating, there seemed to be a positive correlation. However, this correlation was likely do to the fact that longer books would have less overall reviews. This is confirmed with the graph on the right.

### 3d. Average Rating Vs. Total Ratings
Below is the graph for the total ratings over average ratings.

![](https://github.com/R-Yamin/Book_Recommendation_System/blob/master/Saved_images/ratings_count.png)

### 3e. Summary of EDA
>* The mean of the ratings falls around 3.9, meaning most of the rankings for this dataset fall around that area, with few books significantly lower, despite the option to vote for lower rankings.
>* The the most significant relationship for the data is with ratings and the number of rankings given.

## 4. Machine learning
>* [ML Notebook](https://github.com/R-Yamin/Book_Recommendation_System/blob/master/4.%20Machine%20Learning%20and%20Predictive%20Modeling.ipynb)

KMeans clustering helped determine how many categories the books could fall into. Below is the Elbow Curve showing that a k value of 5 (for 5 groups) looked best:

![](https://github.com/R-Yamin/Book_Recommendation_System/blob/master/Saved_images/Elbow_curve.png)

After removing 2 outliers, the distribution of books and the groups looked much cleaner and the data was given labels based on the grouping below.

![](https://github.com/R-Yamin/Book_Recommendation_System/blob/master/Saved_images/Kmeans_clustering_final.png)

## 5. Prediction
With labeled data now, a K-NearestNeighbor model was created to determine which titles were most similar to each other. A k value of 5 was used again since it was shown to consistently perform well with both training and testing data scores. From this model, the top 5 nearest neighbors were given as a list of indices and a function called Books_Recommended was created.

The function Books_Recommended() worked by taking a title given, and returning the top 5 similar books to that book title. Even if a partial title was given, if there were at most 1 copy of the book within the dataframe, it returned a recommended list. A difficulty arose when multiple titles matched a given input. However, this was resolved by giving the user an error message with a list of possible titles and book IDs. The user could then use a book ID to match the book of interest, and the return value would print the title of the book matching the book ID and the top 5 books recommended.

Examples of the function being tested can be found at the end of the ML notebook.
>* [ML Notebook](https://github.com/R-Yamin/Book_Recommendation_System/blob/master/4.%20Machine%20Learning%20and%20Predictive%20Modeling.ipynb)

## Future Improvements
* __This was a toy model based on an already prepared dataset.__ This dataset was already put together from the Goodreads.com API. Future datasets would be more updated and perhaps contain time series data to see when ratings were given. 
* __Incorporating user data.__ With user history and profile information regarding past ratings, it would be possible to strengthen the current recommendation system to tailor it more to individual users rather than to the books themselves.
* __What affect do Authors have?__ With more time and user data, it would likely be possible to see the extent authors play a roll in recommending a book or series.
