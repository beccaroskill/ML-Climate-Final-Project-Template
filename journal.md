
## What is my project?

I would like to use satellite imagery and weather data to predict wildfire occurences.

## My progress

### Monday, March 21th

The main question I'm considering now is how to acquire fire-negative imagery; what should I feed the model as fire-negative regions, against which correctly identifying fire-positive regions will actually be useful? Obviously, if I feed the model pictures of something completely random, say dogs, it's easy enough to distinguish these from fire-positive satellite imagery. But, more seriously, it also feels like cheating to feed the model totally random satellite imagery, when these regions and timestamps might be of no interest to those interested in wildfire risk (say, a highway in the winter).

The intuition seems to be that I would like fire-negative cases to have a similar distribution of preconceived risk as the fire-positive cases. This way, the model will be doing something that existing models would fail to do - turn hazard potential into a more effective prediction. Therefore, the question I have narrowed my study down to is: **Can CNNs reliably predict wildfire occurences given 12-band images where wildfires *did* occur and corresponding 12-band images where wildfires *did not occur*, which were assigned a similar Wildfire Hazard Potential (WHP)?** The more refined a WHP metric I use to determine this distribution, the higher standard I am holding the model to (Does it "beat" these hazard potential models or simply replicate them using a new method?). 

As a starting point, I will use the US Forest Service's Wildfire Hazard Potential for the United States (270-m) (https://doi.org/10.2737/RDS-2015-0047-3). The first step was to sample this data at each of the wildfire's points of occurence, to obtain a fire-positive WHP distribution. Next, I will decide which fire-negative points to obtain imagery for, such that these points have a similar WHP distribution. I suppose I will need to do something similar with other inherent risk factors, like temperature and time of year.


### Wednesday, March 9th

I have nearly completed the process of acquiring the training data I need, and I have made some modifications to my experimental design in the process. I'm having success with acquiring fire-positive imagery - this just entails pulling down imagery in the fire's region preceding the fire's occurence (although even this process has taken much longer than anticipated, because I keep coming across extremely time-consuming issues, like SentinelHub limiting the number of requests from my free account). 

I'll outline the data I have succeeded in acquiring:
- One image per fire
- 200x200 pixel (about 4km x 4km) region around fire's location 
- Cloud cover < 0.1
- Latest-captured acceptable image in the range of 5-14 days prior to fire occurence

### 2022-03-07 check in: alp

Looking good. Would encourage trying to get some initial results soon. Also please try to update this weekly.

### Sunday, February 20th

I resumed my data collection efforts, in an attempt to acquire most (if not all) of the data utilized by Santopaolo et al. I found Google's Earth Engine to be the best tool for pulling Modis data like the fire masks, but I experienced a steep learning curve because the documentation is not great. I should be set in terms of surface reflectance (SR) data because I figured out how to pull down Sentinel2 data a few weeks ago, which is higher resolution than the Modis SR data used in Santopaolo et al. The time resolution is lower, though, so I may need to consider ways to combine the two sources. I also need to decide on a time range (prior to the fire) over which I should use data to make predictions. Santopaolo et al. use 8 days, but with Sentinel2's lower time resolution I may opt for a longer range. Next up, I need to pull down weather data!

### Wedesday, February 16th

I met with Nicolas today to discuss my initial ideas for the project. We talked about [Santopaolo et al. 2021](https://ieeexplore.ieee.org/document/9480226) and the areas I mentioned for improvement (time-dimension methodology, spatial dimension quality). We talked about other ways I could build upon the study, including incorporating a Bayesian model into the prediction tool. In the short term, we made a plan for me to start reproducing Santopaolo et al.'s study, fitting a time-series transformer to account for the time dimension.

### Wedesday, February 9th

After reading the Jain et al survey last week, I was initially interested in [this paper](https://ieeexplore.ieee.org/document/8932740)'s use of satellite data to detect wildfires early on, and I was hoping to investigate whether something similar would be possible for risk prediction. Excitingly, I found one [recent paper](https://ieeexplore.ieee.org/document/9480226) that uses CNNs to perform prediction. The latter paper is pretty similar (maybe too similar) to what I wanted to do, but I am trying to identify aspects that might inform a new approach. Two things that stick out to me as areas for improvemet are 1) their approach of averaging over the temporal dimension of their dataset and 2) the poor spatial resolution of their surface reflectace satellite imagery. I'm going to talk to Nicolas next week to see what he thinks.

### Saturday, February 5th

I initially hoped to use the Kaggle dataset containing 1.88 million wildfires in the US, ranging from 1992 to 2015: https://www.kaggle.com/rtatman/188-million-us-wildfires. However, I ran into a pretty major issue - these wildfires all occured before the time range of open-source Sentinel2 data. 

So instead, I decided to use National Intragency Fire Center GIS data ([2000-2018](https://data-nifc.opendata.arcgis.com/datasets/nifc::historic-perimeters-combined-2000-2018/about); [2019](https://data-nifc.opendata.arcgis.com/datasets/nifc::historic-perimeters-2019/about))

I imported the data and found the geographical position of the fires using geopandas, then I pulled any images captured by Sentinel2 in the 2 weeks preceding a fire using the Sentinel Hub API that I explored earlier. The next step is to do some research into which bands will be useful for my study! 

### Thursday, February 3rd

I read [this survey](https://arxiv.org/abs/2003.00646) of works using ML on various wildfire-related applications. I focused on section 4.3, on "Fire occurrence, susceptibility, and risk," and I found a number of studies that should help me get a good idea of existing work in the field. 

I also took a shot at downloading images from the Sentinel 2 satellite, querying a specific place and time. I used the API's [provided example](https://sentinelhub-py.readthedocs.io/en/latest/examples/data_search.html#Sentinel-Hub-Catalog-API). The exercise is in `./src/InitialExploration.ipynb`, a notebook where I will be running some preliminary efforts.
