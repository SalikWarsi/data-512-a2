# Data 512 Assigment 2
# Bias in English Wikipedia data

The goal of the project is to analyse if their is bias in the english wikipedia articles about politicians from different countries. 

## Data Source
We used the following Data Sources

1. Revision Ids of English Wikipedia Articles about Politicians: https://figshare.com/articles/Untitled_Item/5513449 This is released under a [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/) license. Data from this source was downloaded and saved as **page_data.csv**
2. Population data of countries was obtained from [Population Referece Bureau](http://www.prb.org/DataFinder/Topic/Rankings.aspx?ind=14) This was stored as **Population Mid-2015.csv**

## Data Acquisition

We use [ORES API](https://www.mediawiki.org/wiki/ORES) to get the quality of the Wikipedia articles about the politicians. An example of how we call this API and extract the quality from the JSON is shown in the Jupyter notebook.

We read the page_data.csv  and for each of the Revision Ids in that table we query the ORES API and after parsing the JSON store the result (Quality) in a new list. 

Finally we combine all the data into a pandas data frame and write it to a csv file: **en-wikipedia_politician_article.csv** 

**Note: This takes hours to run so be careful running it.** The stored CSV file is there so we can directly start after this step. This is also why I put it under try-except as I was occaisonally getting an error. However the stored CSV file does contain all the data

## Data Processing
We join the Wikipedia article quality with the population per country using the country as a key. If a country is not present in both data sources we remove its data. Similarly if a politician appears more than once in the table we also remove it to keep our data clean.

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

## License and Copyright
I am publishing under an MIT License.
The data is shared under [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/)

## Tools used
The entire analysis was done using Python 3.5.3 :: Anaconda custom (64-bit)

Libraries used:

1. requests
2. json
3. pandas
4. matplotlib.pyplot
5. csv