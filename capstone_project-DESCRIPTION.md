# IBM Coursera - Capstone Project Report
Capstone project for the Coursera Data Science course.

<a name="intro"></a>
## 1. Introduction/Business Problem
Despite the ongoing global turmoil caused by the spread of the novel coronavirus and the restrictions
of movement that people have to live with to contain the threat, it's safe to say that the people's desire
to travel to foreign countries and cities will be unbroken in the foreseeable future. Travel companies have a huge interest in getting the best possible information and process it in the best possible way to be able to offer their customers helpful and precise advice on their travel destinations. In the modern digital world with all its available information travellers are increasingly demanding and require easily accessible but comprehensive and precise information about their planned journey and destinations. One of the most common type of traveling is a business or non-business city trip, where a traveller is in need of quick orientation in an unknown metropolis with its overwhelming impressions, opportunities, venues and wide variety of unique districts with their individual characters. Wouldn't it be great to help travellers in providing them a useful clustering system for neighbourhoods of each individual city based on several dimensions of up-to-date information? In this project we'll try to offer a solution to this problem.

<a name="data"></a>
## 2. Data
To solve our problem we will use data from http://insideairbnb.com/index.html, a website sourcing
publicly available information from the Airbnb site. This data includes listings of accomodations of more than hundred cities worldwide, along with geo data of the cities' neighbourhoods. We will use the price and room type information of the listings data along with venues information from the Foursquare API to build a meaningful and characteristic feature set for each neighbourhood of a specific city and then apply a K-Means model to gain clusters of similar city districts.
