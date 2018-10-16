---
layout: post
title: All about the Beds and Baths
---
### Determining the price of an Airbnb rental in Chicago and the factors that influence it
![Airbnb](/images/blog3/pexels-photo-276724.jpeg)
Photo from Pexels

Airbnb has been growing in popularity over the years - to a point now - where it has emerged as a viable alternative to hotels. It is especially alluring to travelers who are looking for cheaper yet comfortable accommodations - while looking to experience a distinct local flavor. And the popularity shows.

A quick check of the search trends for the Airbnb website [Google Trends](https://trends.google.com/trends/?geo=US) shows a steady increase in its popularity over the past five years.

(*An aside: watch this space for a companion piece on time-series modeling of the Google trends data using Facebook's Prophet.*)

<script type="text/javascript" src="https://ssl.gstatic.com/trends_nrtr/1544_RC05/embed_loader.js"></script> <script type="text/javascript"> trends.embed.renderExploreWidget("TIMESERIES", {"comparisonItem":[{"keyword":"/m/0svqyn7","geo":"US","time":"today 5-y"},{"keyword":"/m/025ypk","geo":"US","time":"today 5-y"}],"category":0,"property":""}, {"exploreQuery":"date=today%205-y&geo=US&q=%2Fm%2F0svqyn7,%2Fm%2F025ypk","guestPath":"https://trends.google.com:443/trends/embed/"}); </script>
Google Trends data on the web searches for the Airbnb website for the past five years.

So as a part of the second project in the [Metis Data Science Bootcamp](https://www.thisismetis.com/data-science-bootcamps?gclid=EAIaIQobChMI49W6ofuJ3gIVFY7ICh3aFg4QEAAYASAAEgJsevD_BwE), I worked on getting data about Airbnb listings in the Chicago area to try and predict what factors influence their pricing.

### Scraping the data

>In 2017, three young entrepreneurs started what was at the time called Airbedandbreakfast.com. Since then, Airbnb has not only shortened its name, but has expanded to more than 34,000 cities and as of 2016, had been used by more than 60 million guests. The company is currently the second-highest valued startup in the U.S. at $31 billion.

>[Business Insider](https://www.businessinsider.com/brian-chesky-airbnb-ceo-life-story-photos-2017-7#along-with-a-third-cofounder-nathan-blecharczyk-gebbia-and-chesky-started-what-was-at-the-time-called-airbedandbreakfastcom-8)

The $31 billion kitty for Airbnb comes from their commission ranging from 3-12% of the rental price depending on the listings. But, what features of the host's offering helps set the price of their rentals (and which features dont) was the main aim of my analysis.

To identify some of the features to model the price of the rental, i turned to what the host's thought were the top selling points of their offering that would entice a potential customer to choose them. I scrapped their descriptions and created a [wordcloud](https://github.com/amueller/word_cloud) to identify the most common features advertised.

![wordcloud](images/blog3/name_wc.png)
The most important features in the host's own words.

Interestingly, some of the main features that are highlighted here are the bedrooms of the rental offerings, which could be private rooms, apartments or condos, and entire houses. Another key selling points seems to be the location of the property.

Based on this analysis and a study of almost 51 different features offered by the host, I whittled the list down to 10 variables that could best predict the rental price using the Variance Inflation Factor (for an explanation of the VIF and how it was used to select features for predicting how drugs are prices, read the blog by the King of Scraping, [Charlie Yaris](https://cyaris.github.io/)).

### Is the price right?

![correlation plot](images/blog3/correlation_plot.png)
10 best features used to predict the price of an Airbnb rental.

The features and the price of the rental were fed into a linear regression model to see if they could predict the price of the rental. The resultant model was not one of the best, as the model scores (R^2, for the technical audience) indicated, but there were some interesting takeaways from the analysis.

Firstly, the number of bedrooms and baths clearly had the biggest influence on the price of the rental. For every bedroom offered, the host could charge $23 more per night. So, $46 for two bedrooms, $69 for three and so on. Add to it $10 more per bath added. As a result, entire homes and condos fared better than private rooms.

![beds and baths](images/blog3/room_type_price.png)
Entire homes pulled in a bigger share of rental prices compared to private rooms.

Although the location of the rental seems to matter in the preliminary analysis, the model did not corroborate that observation. One possibility the median rental prices in a zipcode I used as a numerical proxy for the role of a location in rental pricing may not have been the best measure. However, I did get some interesting results about rentals along transit routes in Chicago, which will be the subject of another blog! So stay tuned!

### The curse of a review?

Superhosts are hosts who have been offering up their homes for a considerable period of time, and have consistently earned good reviews. One would assume that these superhosts could demand a better price for their rental compared to normal hosts. But, that was not the case!

![superhost](images/blog3/superhost_price.png)
Highly reviewed superhosts do not earn more.

This trend was evident in the correlation plot which also showed that the number of reviews a rental received did not track the price of that rental. However, why that would be the case, is still unclear.

### Tools used for the project

* Python : for most of the cleaning and analysis
* Pandas dataframes
* Beautiful Soup
* Selenium
* Wordcloud

Check out [my GitHub repo]() for more details regarding the project, codes and results!

### Next Steps

Off to week four and classification problems with SQL! Keep an eye on this blog for more updates in the coming weeks! I leave you with some interesting quotes from some truly learned people!

### Interesting Quotes

>"It has become appallingly obvious that our technology has exceeded our humanity."- Albert Einstein

>"Never trust a computer you can’t throw out a window."- Steve Wozniak

>"Simplicity is about subtracting the obvious and adding the meaningful."- John Maeda
