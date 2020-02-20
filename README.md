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
![](
