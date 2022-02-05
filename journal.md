
## What is my project?

I would like to use satellite imagery and weather data to predict wildfire occurences.

### Thursday, February 3rd

I read a survey of works using ML on various wildfire-related applications (Jain et. al, 2020. "A review of machine learning applications in wildfire science and management"). I focused on section 4.3, on "Fire occurrence, susceptibility, and risk," and I found a number of studies that should help me get a good idea of existing work in the field. These have been uploaded to `./etc/bib/`, where I will be storing the papers I check out :)

I also took a shot at downloading images from the Sentinel 2 satellite, querying a specific place and time. I used the API's provided example: https://sentinelhub-py.readthedocs.io/en/latest/examples/data_search.html#Sentinel-Hub-Catalog-API. The exercise is in `./src/InitialExploration.ipynb`, a notebook where I will be running some preliminary efforts.

### Saturday, February 5th

I initially hoped to use the Kaggle dataset containing 1.88 million wildfires in the US, ranging from 1992 to 2015: https://www.kaggle.com/rtatman/188-million-us-wildfires. However, I ran into a pretty major issue - these wildfires all occured before the time range of open-source Sentinel2 data. 

So instead, I decided to use National Intragency Fire Center GIS data:
- 2000-2018: https://data-nifc.opendata.arcgis.com/datasets/nifc::historic-perimeters-combined-2000-2018/about
- 2019: https://data-nifc.opendata.arcgis.com/datasets/nifc::historic-perimeters-2019/about

I imported the data and found the geographical position of the fires using geopandas, then I pulled any images captured by Sentinel2 in the 2 weeks preceding a fire using the Sentinel Hub API that I explored earlier. The next step is to do some research into which bands will be useful for my study! 
