# Properties-Project---Edinburgh
An insight into the Edinburgh property market

### PROJECT SUMMARY 
Last year I decided to start a project to study the house market prices in Edinburgh. Just having bought my first property, I found the process of knowing how much to bid to secure a property challenging and frustrating at times. 

My intention was to develop a system that would allow me to monitor, over time, the fluctuation of prices in the different areas of Edinburgh, as well as how much under/over the asking price the properties sell for. In addition, I also recorded information about the  Estate Agents involved in the sales for further assessment, as this information was available in the listings. 

I have been collecting data for over 8 months - approximately 1K entries. Even though this amount of information doesn’t provide yet enough backing to present definitive conclusions, it is possible to start identifying some tendencies and therefore I have decided to publish the first batch of results.The findings are presented through simple and easily understandable visualizations. The intention is to build on this idea and apply a similar approach to other cities in Scotland and the UK.

### MAIN FINDINGS
<dl>
  <dt>Some of the key insights that this project has started building are:</dt>
</dl>

1. What is the average asking price range by postcode sector (this is to the EHXY Z## level of detail)
2. How much over or under the asking price, in average, are properties sold for on each sector.
1. Which are the most/less popular areas of Edinburgh where properties are being listed for sale.
2. What are the usual range of different Estate Agents listings? Is there a difference - for instance some agents specialising in cheaper/more expensive properties?
2. What are the sold prices achieved, in average, by different Estate Agent? Would it be expected a difference if a property is listed with different Agents?


<img width="1302" alt="Properties asking prices" src="https://user-images.githubusercontent.com/70529856/91867653-cb054f00-ec6b-11ea-8f05-0fca4b08db83.png">
<img width="1141" alt="Properties sold prices over-under" src="https://user-images.githubusercontent.com/70529856/91868229-6dbdcd80-ec6c-11ea-9277-32a07e09cb62.png">

<p align="center">
 POPULAR AREAS FOR PROPERTIES LISTINGS
<img width="992" alt="Properties listings locations" src="https://user-images.githubusercontent.com/70529856/91867997-29cac880-ec6c-11ea-9c58-da078c0e3453.png">
</p>

![Estate Agents Asking Price](https://user-images.githubusercontent.com/70529856/91870111-8c24c880-ec6e-11ea-9909-a3f829d3a1bf.png)
![Estate Agents OverUnder](https://user-images.githubusercontent.com/70529856/91870113-8dee8c00-ec6e-11ea-844d-714c1f082bff.png)

## PROJECT BREAKDOWN:
## 1-PROBLEM STATEMENT 

Like in many other places, the process of purchasing a new house works in a way that houses are advertised for a certain price (Asking Price). This is usually around the value set up in the Home Report (HR value), which provides an estimate for the market value of the property. With this in mind, people are free to bid whatever they wish to secure the place. Usually the highest bidder is the one that ends up securing the place, defining the Sold Price for the property. In an upwards trend market, you would always be expected to offer over the Asking Price.

Therefore, after attending a viewing for a property that we liked and reviewing its HR, it always arose the question: how much should (and can) we offer for this place in order to secure it?

This was not straightforward because, in order to find the right answer, I would need to have information on previous Asking Prices for properties nearby, and the Sold Price achieved by them. That would give us information on similar properties in terms of size and location that we could extrapolate to ours. However, this information was not easy to get since those two prices would not be available at the same time. The Asking Price is available while the property is still up for sale, whereas the Sold Price is shown only after all the legal procedures are finalised and this is at least 3 to 4 months after the sale takes place when the Asking Price has long been removed. Also, the differences between Asking Prices and Sold Prices vary considerably between different areas of the city - but this information is not available to the buyer.

In addition, information about Estate Agents has also been gathered. I find this insight particularly useful when it comes to sell a property, as with time, it might start giving an indication on which Estate Agent is stronger for a given price range, or even a particular area. And whether there is a difference in the final sold price of the property.

## 2-SOLUTION FUNCTIONALITY

When developing the solution for this project, I first tried to find out whether there would be an API that I could connect to in order to get the data in an orderly manner. However, at the time of research, early 2019, I couldn't find a working/maintained one. Therefore I used the libraries 'Requests' and 'Beautifulsoup' together with a lot of data cleaning to gather the data as properties would become available to the market. Then, I would try to match them with their sold price once this information becames available - usually a couple months after the sale has been finalise. 

In order to run the scripts daily and weekly an online server to host and run python code on the cloud has been used (PythonAnywhere), allowing to pre-schedule the frequency with which the code should run. All the results are stored in a .csv file from which visualizations can be pulled at anytime. The F=figure below shows this working arragement:

<p align="center">
<img width="675" alt="Functionality" src="https://user-images.githubusercontent.com/70529856/91943681-e2d4e580-ecf4-11ea-89c2-98142e764294.png">
</p>

The functionality of both the ListingScraper and SoldPriceScraper is similar and it is outlined below.

#### 2.1-ListingScraper

The information recorded for each property would include:
1. The date when the property was posted
1. The number of beds 
1. The asking price 
1. The Estate Agent that carrying out the sale 

<p align="center">
  ZOOPLA LISTING (BOTTOM) AND DATAFRAME OUTPUT (TOP)
<img width="700" alt="ListingScraper example" src="https://user-images.githubusercontent.com/70529856/91957236-aa85d500-ecfd-11ea-8a37-26165d1157ef.png">
</p>

From this information two additional features are created: 
1. The first one is the coordinates of the property (LAT and LON) using Google's API and based on the address obtained. This is used later to create the visualizations. 
1. The second one is a new column called 'Address input' which is used by the 'SoldPriceScraper' to search on Zoopla for sold prices at a later date.

#### 2.2-SoldPricesScraper

The sold price scraper uses the 'Address Input' column to run a query on the server and search for sold properties under that same address. This returns a list of properties, however since the 'Address Input' uses the same naming as Zoopla for the address, if the sold price is available, it will be the first one on the list. 

There were, however, some instances in which this method gave problems, for instance those occasions where the property might not have been sold, or removed from sale. In this case the Scraper would pick the next property list, which led to wrong matching. To correct this, two measures were introduced:

1. Check that the sold price and asking didn't diverge by more than 50% (which would not be realistic)
1. Check that differences between the listing date published and the sold date was not higher than 6 months (higher end of the timescales for other properties)

The SoldPricesScraper runs once a week and iterates through all the entries in the .csv which do not have an allocated 'Sold Price'

#### 2.3-EdinburghFinal.csv

All the information gathered from the scrapers above is stored in a cloud service on a .csv file. The final output produced by those two scrapers is like the figure below

<p align="center">
  EXTRACT OF .CSV PRODUCED BY THE TWO SCRAPERS
<img width="1289" alt="Final csv" src="https://user-images.githubusercontent.com/70529856/91992992-bf2d9180-ed2c-11ea-8bfd-87b5ea9c53e8.png">
</p>

#### 2.4-Visualizations

The .csv above allows to produce a number of visualization. In order to produce the visualization and show the results by postcode it is necessary to use a shapefile with the postcodes sectors within the UK (also included). The notebook attached shows the 5 visualizations provided above, however further exploration can be performed using the number of beds or the dates where the listing was posted (time series). This is left for when more data is available.

## 3-CONCLUSIONS

It is important to remark that these visualizations are built on data acquired over a limited amount time. The system will prove more valuable as more data is added, allowing to build insights over time. However, there are some trends which can be observed already. Future data will confirm whether these are correct tendencies or otherwise. Some general conclusions ca be:

1. The market still seems to be in an upward trend from Spring 2019 to Winter 2020, when this batch of data was collected, shown by the transparent/red shades the in Percentage Paid Over/Under figure. However, there are already some areas, mostly on the outskirts of Edinburgh, which seem to have a lower demand and falling prices (shown in blue shades in the Percentage Paid Over/Under figure). 
1. Some of the areas  where more flats are listed for sale include Leith ((EH6 and EH7 and surrounding neighbourhoods) and Dalry and Gorgie (EH11), which are also on the lower end of the price range (100 to 200K).
1. If the insights from 1 and 2 are put together, there might be an interesting observation when it comes to buy a new property: even though the asking prices in the popular areas of Leith and Dalry are lower, the percentage over Asking Price paid in these areas can go up to 10% or even 20% in areas of Leith (EH6 5). This could mean that properties in the immediate next upper bound of Asking Prices (say 150 to 250K) but where the prices are going under the Asking Price value (some EH4, EH5 and EH15 for instance) could be a better investment at the moment. This is because with a similar amount of deposit you could be buying a higher valued property, insted of offering over the Asking Price to secure it in Dalry or Leith.
1. On the agents side of things, only those with over 10 properties sold have been selected to make sure that there is some basis on the observations. It can be seen how big players like Warners seem to specialise in the mid-range price level of properties, with an average sale price of 150K approx and similar to Neilsons. On the other end, Balfour+Manson and Coulters seem to have a considerably higher average Asking Price, at almost 300K.
1. Some interesting tendencies are observed from the last figure, where it seems that agents like Warners seem to achieve approximately a 5% increase in sold price over the asking price, whereas others like Balfour+Manson seem to increase this to a 15%. This might reveal some underlying pattern between different Estate Agent including different selling techniques. However, as these two Agents seem to specialise in lowest and highest ends of the Asking Price range, it might also mean that at the moment, the properties achieving higher % increase over Asking Price are the more expensive ones. 

## 4-FURTHER WORK
There are a number of ways in which this system could be improved and expanded. Below, I list a number of actions that I have identified as the more urgent ones:
1. Develop an updated SoldPriceScraper: the current one to use in Zoopla is currently not working. I am exploring the possibility of using the API if it is available this time.
1. Create better and more interactive visualizations: at the moment these are Matplotlib, Seaborn and Geopandas based. However, there are some really nice tools like Bokeh, which allows to produce more visual ones with more information to display. 
1. Expand this system to other cities in Scotland/UK: the ultimate goal would be to have a database publicly available with these insights that people could use to gain information to assisst in the stressful time of buying a house. I have already started working in making more flexible code so that new cities can be added easily.


I really hope you have enjoyed this project. If you wish to collaborate, please do not hesitate to get in touch!!





