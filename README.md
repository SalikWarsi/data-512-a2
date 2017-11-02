# Data 512 Assigment 2
# Bias in English Wikipedia data

The goal of the project is to analyse if their is bias in the english wikipedia articles about politicians from different countries. 

## Data Source
We used the following Data Sources

1. Revision Ids of English Wikipedia Articles about Politicians: https://figshare.com/articles/Untitled_Item/5513449 This is released under a [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/) license. Data from this source was downloaded and saved as **page_data.csv**. Note: While creating this data set, the developer only performed two level recursion. Therefore a politician listed as an American politician is included but a politician listed as an "American politican who was Assassinated" but not "An American Politian" is not. Also this data includes articles like List of Politicians as well such as [Template:ZambiaProvincialMinisters](https://en.wikipedia.org/wiki/Template:ZambiaProvincialMinisters) 
2. Population data of countries was obtained from [Population Referece Bureau](http://www.prb.org/DataFinder/Topic/Rankings.aspx?ind=14) This was stored as **Population Mid-2015.csv**. I could not find the license for this data. However PRB aims to provide data for public use.

## Data Acquisition

We use [ORES API](https://www.mediawiki.org/wiki/ORES) to get the quality of the Wikipedia articles about the politicians. An example of how we call this API and extract the quality from the JSON is shown in the Jupyter notebook included in this repository.

We read the page_data.csv  and for each of the Revision Ids in that table we query the ORES API and after parsing the JSON store the result (Quality) in a new list. 

Finally we combine all the data into a pandas data frame and write it to a csv file: **en-wikipedia_politician_article.csv** .
Its columns are:

| Column       | Values                                                                                  |
|--------------|-----------------------------------------------------------------------------------------|
| politician   |  Name of the politician/article (String)                                                |
| article_name | Name of the Country the politician belongs to (String)                                  |
| revid        | Revision ID of the Last edit to this Wikipedia Article about the politician (int64)     |
| quality      | Quality of the Article as returned by ORES. Can be (FA, GA, B, C, Start, Stub) (String) |

**Note: This takes hours to run so be careful running it.** The stored CSV file is there so we can directly start after this step. This is also why I put it under try-except as I was occaisonally getting an error. However the stored CSV file does contain all the data

## Data Processing
We join the Wikipedia article quality with the population per country using the country as a key. If a country is not present in both data sources we remove its data. Similarly if a politician appears more than once in the table we also remove it to keep our data clean.

This is done by counting number of rows per revision id and only keeping those rows which have only 1 row per revision id. I chose revision id as the unique identifier and not name as i

We finally write it to **PoliticiansArticleQualityWithCountryPopulation.csv**

Its Schema is:

| Column          | Values                                                                                  |
|-----------------|-----------------------------------------------------------------------------------------|
| country         | Name of the Country the politician belongs to (String)                                  |
| article_name    | Name of the politician of this country (String)                                         |
| revision_id     | Revision ID of the Last edit to this Wikipedia Article about the politician (int64)     |
| article_quality | Quality of the Article as returned by ORES. Can be (FA, GA, B, C, Start, Stub) (String) |
| population      | population of the country (int64)                                                       |

## Data Analysis
We perform some analysis in the data as shown in the Python notebook.
We produce the following images:
1. **Bottom10ArticlesPerCapita.png**
2. **Top10ArticlesPerCapita.png**
3. **Top10QualityArticleRatio.png**

We show how we feel there is a bias in coverage of politicians from different countries.

I first showed how the ratio of number of politician articles and the population varies by country by showing the top 10 and bottom 10. We can see the highest ranked countries for those are some of the smallest countries in population while most of the countries at the bottom of this ranking have a high population. Even so countries like USA which have a very high population (3rd highest) still don't figure at the very bottom while some countries like Uzbekistan ( which has a realtively small population) still have a very low ratio. This could indicate bias against countries like Uzbekistan indicating that they might have lesser number of politicains listed in English Wikipedia than they should.

However it is important to note that the size of a country's parliament (or other governing body) which might indicate number of famous politicians is not directly proportional to their population. For example Tuvalu's population is less than 12000 while that of India is more 1.2 billion. However, as seen in https://en.wikipedia.org/wiki/Parliament_of_Tuvalu while their parliament has 15 members, India's doesn't have 150,000 members of parliaments. This could indicate why smaller countries tend to have higher ratios.

However then we comparedhow many of the proportion of articles were rated as GA or FA (i.e. what we counted as a high quality article) by ORES. Here we see that while for some countries almost 1/8th of the articles about their politicians have a high quality, for many others there are 0 articles that have a high quality. This indicates the imbalance in coverage in English Wikipedia articles and shows bias

## License and Copyright
I am publishing under an MIT License. which is included in the repository

The data is shared under [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/)

## Tools used
The entire analysis was done using Python 3.5.3 :: Anaconda custom (64-bit)

Libraries used:

1. requests
2. json
3. pandas
4. matplotlib.pyplot
5. csv

These are standard libraries and can be installed using anaconda default setup. Or alternatively using "pip install"