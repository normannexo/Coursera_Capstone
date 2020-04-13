# IBM Coursera - Capstone Project Report
Capstone project for the Coursera Data Science course.

### Clustering neighbourhoods

![alt text](https://raw.githubusercontent.com/normannexo/Coursera_Capstone/master/img/london_map_clusters.JPG)


### Table of content
1. [Introduction](#intro)
2. [Data](#data)
3. [Methodology](#methods)
4. [Results](#methods)
5. [Discussion](#methods)
6. [Conclusion](#methods)

<a name="intro"></a>
## 1. Introduction/Business Problem
Despite the ongoing global turmoil caused by the spread of the novel coronavirus and the restrictions
of movement that people have to live with to contain the threat, it's safe to say that the people's desire
to travel to foreign countries and cities will be unbroken in the foreseeable future. Travel companies have a huge interest in getting the best possible information and process it in the best possible way to be able to offer their customers helpful and precise advice on their travel destinations. In the modern digital world with all its available information travellers are increasingly demanding and require easily accessible but comprehensive and precise information about their planned journey and destinations. One of the most common type of traveling is a business or non-business city trip, where a traveller is in need of quick orientation in an unknown metropolis with its overwhelming impressions, opportunities, venues and wide variety of unique districts with their individual characters. Wouldn't it be great to help travellers in providing them a useful clustering system for neighbourhoods of each individual city based on several dimensions of up-to-date information? In this project we'll try to offer a solution to this problem.

<a name="data"></a>
## 2. Data
To address our problem we will use data from http://insideairbnb.com/index.html, a website sourcing
publicly available information from the Airbnb site. This data includes listings of accomodations of more than hundred cities worldwide, along with geo data of the cities' neighbourhoods. We will use the price and room type information of the listings data along with venues information from the Foursquare API to build a meaningful and characteristic feature set for each neighbourhood of a specific city and then apply a K-Means model to gain clusters of similar city districts.

Data used in the project:
1. Airbnb data from http://insideairbnb.com/index.html:
- `listings.csv` Summary information and metrics for listings in a specific city
- `neighbourhoods.csv`
- `neighbourhoods.geojson` GeoJSON file of neighbourhoods of the city, mainly used for data visualizations
2. Foursquare API to retrieve the venues for the neighbourhoods of a specific city.



<a name="methods"></a>
## 3. Methodology
We will use the web scraping library Beautiful Soup to gather data from the insideairbnb archive. Insideairbnb offers geo information, listings, calendar and reviews data for each of
the currently 100 cities in the archive. We are especially interested in the most recent listings and neighbourhood data, so our defined helper functions take a list of specific cities and downloads the most current lisitings.csv, neighbourhoods.csv and neighbourhoods.geojson to subfolders.
Our starting point for building up our data set for the is the `listings` file. We take the data from this file, group it by neighbourhood and
collect the first features for our subsequent analysis:
- airbnb_price_level: We use the average price values for each neighbourhood as an overall price indicator. To be able to compare price levels to other cities we additionally normalize the price data.
- room types: We calculate the distribution of offered room types (private room, shared room, hotel room, etc.) in each neighbourhood and store them in extra columns to use them as a feature for creating our clusters in later steps.

We also use the longitude and latitude of each listing to calculate mean values for each neighbourhood, which we use later on to retrieve venue data via the Foursquare API.

#### Exploring via Foursquare API

Foursquare uses a hierarchical category system and we will use the top level category of each venue for our analysis. So we have to get all categories first and
store them into an extra data frame to look up the top level category for our venue search results later.

We then fetch venue data for all neighbourhoods for the city in question and apply 3 operations to our result list:
1. mapping the top level category to each venue by looking up the venue category in our foursquare category table
2. tidy up the data and get rid of abundant and unnecessary data
3. create dummy columns for all categories
4. group the data by neighbourhood.

So now we have collected all features being used for the creation of helpful clusters:
- price level (with the airbnb_price_level as an indicator)
- distribution of room types (as an indicator which type of rooms prevail in certain clusters)
- distribution of a neighbourhoods venue categories

##### Clustering

We are now able to apply machine learning algorithms to our data and will use k-means for our clustering since it is a powerful
and proven algorithm for partitioning data into clusters. We apply preprocessing to our data by scaling it with the Scikit-Learn's StandardScaler object.
We try to calculate an optimal number of clusters by applying the elbow method.
After applying the algorithm we merge our resulting cluster labels to our data.

We then use visualizations like the following to derive specific characteristics from our clusters:
![alt text](https://raw.githubusercontent.com/normannexo/Coursera_Capstone/master/img/img1.JPG)


<a name="results"></a>
## 4. Results

### Getting data for London
In our project we used the data for the city of London as an example. We got data for 33 neighbourhoods from the airbnb listings data.
We retrieved 888 venues from Foursquare which were distributed to venue categories as shown below:
![alt text](https://raw.githubusercontent.com/normannexo/Coursera_Capstone/master/img/venues_cat.jpg)

### Clustering data
The elbow method for determining an optimal value for k gave the following result:
![alt text](https://raw.githubusercontent.com/normannexo/Coursera_Capstone/master/img/elbow.JPG)

No unequivocal value was found, so we used $$k=4$$ for our model.

We achieved the following cluster map of London:
![cluster map](https://raw.githubusercontent.com/normannexo/Coursera_Capstone/master/img/london_map_clusters.JPG)

To get an understanding for the caracteristics of each cluster, we plotted barcharts with the average value
for each feature by cluster:
![avg features](https://raw.githubusercontent.com/normannexo/Coursera_Capstone/master/img/avg_cluster.jpg)

So we get the following major insights into our clusters:

- **Cluster 0** (e.g. Westminster, City of London): high price level, few private rooms, many restaurants
- **Cluster 1** (e.g. Brent, Newham): relatively low price level, many private rooms, many shops
- **Cluster 2** (e.g. Bromley, Croydon): low price level, many private rooms, many outdoors & recreation venues
- **Cluster 3** (e. g. Camden, Islington): moderate price level, many private rooms, many nightlife venues


<a name="discussion"></a>
## 5. Discussion
In the previous sections we developed a promising approach to a helpful clustering of city data that might be
useful for the customers of travel companies when planning city trips. We focussed on specific features of
venue data of the neighbourhoods and used certain properties of airbnb listings as indicators for price level, etc.
Certainly, it would be useful to elaborate more neighbourhood features like transportation services to make the model even more precise.

<a name="conclusion"></a>
## 6. Conclusion
Our approach to develop a clustering model for cities seems to be good 