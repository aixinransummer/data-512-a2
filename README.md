# data-512-a2 Bias in Data

# Description and Goal
This project aims to explore potential bias in data on Wikipedia articles of political figures from different countries in the world. By observing the pattern of two metrics articles-per-population rate and high-quality-articles rate across countries and sub-regions, we reflected about the impact of limitation of data and bias throughout the analysis process on the interpretation of the results. 

# Project Details

## Data Acquisition
There are two datasets used for this project. 

The first one can be found on [Figshare](https://figshare.com/articles/dataset/Untitled_Item/5513449), which provides Wikipedia politician articles of different countries. The [page_data.csv](page_data.csv) has three columns in total: page, country, and rev_id. The page column refers to the name of the article and the rev_id is an id that can be used later for the assessment of articles. 

The second datasets contains population information by countries and regions in the world. The [WPDS_2020_data.csv](WPDS_2020_data.csv) can be obtained from the [world population data sheet](https://www.prb.org/international/indicator/population/table/) published by the Population Reference Bureau. This dataset has six columns, which are FIPS, Name, Type, TimeFrame, DATA(M) and Population. The 'FIPS' and 'Name' columns are the abbreviation and full name of the countries or regions being referred to. The 'Type' attribute identifies whether the current place is a country of a sub-region (Eg. Africa). The 'TimeFrame' columns tells us when the data was collected and the last two attributes provide population information in different units. In addition, one can identify which country belongs to which sub-region by looking at the name of the sub-region that appears beforehand. 

## Data Cleaning and Data Processing
In the first step, rows in the page_data.csv that start with 'Template:' were excluded. As mentioned before, there are rows in the WPDS_2020_data.csv that provide cumulative regional population counts rather than country-level counts. Therefore, all the sub-region rows in the dataset, identified by being all capitalized letters in the 'Name' column, are retained separately so that country-level rows can be joined with page_data.csv later.  

### Getting Article Quality Predictions
We used a machine learning tool called ORES to estimate the quality of Wikipedia articles in the page_data.csv. From best to wrost, the esitmated qualities are:
1.	FA - Featured article
2.	GA - Good article
3.	B - B-class article
4.	C - C-class article
5.	Start - Start-class article
6.	Stub - Stub-class article

The algorithm was trained based on articles in Wikipedia that were peer-reviewed using the [Wikipedia content assessment](https://en.wikipedia.org/wiki/Wikipedia:Content_assessment) guideline. To get the predicted score for each article, we configured the corresponding revision ID (rev_id column) in the [ORES REST API](https://ores.wikimedia.org/v3/#!/scoring/get_v3_scores_context_revid_model) endpoint. There are articles that have no predicted scores and will be excluded from the analysis in the next few steps. 

### Combine the Datasets
The three datasets (article dataset, population dataset, prediction scores for each revision ID) are joined together to form the final dataset. In this step, there will be rows from the three datasets that can not be matched. For example, there are mismatched countries between the article dataset and the population dataset, and there are articles with no scores as mentioned before. All the no-match records is stored in the [wp_wpds_countries-no_match.csv](wp_wpds_countries-no_match.csv) and the final datasets is stored in the [wp_wpds_politicians_by_country.csv](wp_wpds_politicians_by_country.csv). There are five columns in the final dataset: page, country, rev_id, Population and prediction. 

## Data Analysis
Two metrics are computed for countries and regions in the analysis step. The first metric is called articles-per-population rate, which is the number of total articles of a country(region) divided by the country's(regions) own population. The second metric is called high-quality-articles rate, which is the proportion of FA or GA articles among all politician articles in a specific country(region). 

## Results
There are six tables in total listed as final results. The first four tables contain the top and bottom 10 countries by articles-per-population rate and high-quality-articles rate. The last two tables rank the regions in descending order of the two metrics as well. By observing the results in the six tables, we reflected on the potential limitation and bias in the data in the [jupyter nobebook](hcds-a2-bias.ipynb).

## License
Licensed uder the [MIT License](LICENSE),


