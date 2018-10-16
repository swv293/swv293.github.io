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

So as a part of the second project in the [Metis Data Science Bootcamp](https://www.thisismetis.com/data-science-bootcamps?gclid=EAIaIQobChMI49W6ofuJ3gIVFY7ICh3aFg4QEAAYASAAEgJsevD_BwE), I worked on getting data about Airbnb listings in the Chicago area to try and predict what factors influence their pricing.

### Scraping the data

>In 2017, three young entrepreneurs started what was at the time called Airbedandbreakfast.com. Since then, Airbnb has not only shortened its name, but has expanded to more than 34,000 cities and as of 2016, had been used by more than 60 million guests. The company is currently the second-highest valued startup in the U.S. at $31 billion.
>[Business Insider](https://www.businessinsider.com/brian-chesky-airbnb-ceo-life-story-photos-2017-7#along-with-a-third-cofounder-nathan-blecharczyk-gebbia-and-chesky-started-what-was-at-the-time-called-airbedandbreakfastcom-8)

The $31 billion kitty for Airbnb comes from their commission ranging from 3-12% depending on the listings.

Given that WYWT would hold their gala in summer, we pulled out the turnstile data for the months preceding it, from March to May and used it for our analysis.

Based on our cleaned-up data (maybe I should go over how we did all that in another blog post?), we came up with a list of top stations with the highest footfalls. We focussed on the number of exits from any given station, as we inferred that people would be more amenable to chatting with volunteers at their destination.

![top stations](images/stations_by_exits.png)
Top MTA stations based on the turnstile Data

### The income angle

![distribution](images/Exit_map.png)
GPS locations of the top MTA stations that we selected. Larger circles indicate more foot falls.

WYWT is looking to expand on its donor base through the street campaign, so we decided to add income data from Kaggle ([household income data](https://www.kaggle.com/goldenoakresearch/us-household-income-stats-geo-locations)), to supplement the footfall data and get more meaning into our analysis.

We identified the zipcodes around our top stations and mapped the income distribution around these locations.

![income distribution](images/Income_map.png)
GPS locations of the income distribution. Darker circles indicate higher incomes.

We also devised a simple algorithm that mapped both footfall and income data into a single function, adding weights to each component so that we could tweak the recommendations towards either income heavy stations or footfall heavy stations, based on the client's preference. For more on that checkout my cohort-mate and project team member Tim Bowlings's blog [here](https://extraordinaryleastsquares.com/data-science/).

![income and footfall](images/Rank_Map.png)
GPS map of top ranked MTA Stations based on the combined income and turnstile data.

### Our recommendations

* List of top fifty stations based on our combined income and footfall ranking system.

* Mondays and Fridays are the best days for positioning street teams at the exits of the stations to maximize their impact as the ridership peaks on those days.

![Exits and the day of the week](images/exits_week.png)
The turnstile data was separated by the days of the week and aggregated over the entire period of 13 weeks to give rise to the distribution of exits over the days of the week for selected stations with the shadow being the 95% confidence interval around each line plot.

Note: Wednesday traffic seem to be fluctuating more than any other day for the period we analyzed.

### Tools used for the project

* Python : for most of the cleaning and analysis
* Pandas dataframes
* Beautiful Soup
* Selenium
* Folium
* Wordcloud

### Next Steps

Off to week four and classification problems with SQL! Keep an eye on this blog for more updates in the coming weeks! I leave you with some interesting quotes from some truly learned people!

### Interesting Quotes

>"It has become appallingly obvious that our technology has exceeded our humanity."
- Albert Einstein

>"Never trust a computer you can’t throw out a window."
- Steve Wozniak

>"Simplicity is about subtracting the obvious and adding the meaningful."
- John Maeda
