# Final Project
## The Geography of Urban Form in America (and abroad)

This project will use raster and vector data on the form of cities to compare urban core morphologies across America. To do this, I will aggregate street networks, building footprints, vegetation and population from a combination of open source resources. The result will be a similarity score for cities, focusing on extracts of downtowns, allowing users to compare built environments across the country, along with twin cluster analyses to build categories of these environs.     

1. [datasets](#datasets)
2. [questions](#questions)
3. [methods](#methods)
4. [requirements](#requirements)

## datasets

The bulk of these data will come from **osm** via the **osmnx** interface. Building on [past](https://geoffboeing.com/2019/09/urban-street-network-orientation/) and [recent](https://geoffboeing.com/2020/11/off-grid-back-again/#more-5182) work from Geoff Boeing, I will determine if a city is flat or hilly, gridded or messy, dense or sparse, paved or planted, and how busy it gets at day and at night.

#### Measures of Urban Form
![](https://raw.githubusercontent.com/asrenninger/wrangling/master/viz/morphology.gif)

I will also compute how repetitive a city using a measure called *fractal dimension*, which roughly captures how much a city at a given scale is like itself at another scale: does a city look the same if you take bigger or smaller tiles of it?  

#### Fractal Dimension by Buildings and Streets
![](https://raw.githubusercontent.com/asrenninger/wrangling/master/viz/fractal-dimension.png)

[Oak Ridge National Laboratory](https://geoplatform.maps.arcgis.com/home/item.html?id=e431a6410145450aa56606568345765b) provides information on daytime and nighttime population at a resolution of 100 meters, which we can use to estimate how busy an area is—and whether or not people are concentrated on a few pixels or many, which also helps describe the character of an area.   

#### Day-Night Variations
![](https://raw.githubusercontent.com/asrenninger/wrangling/master/viz/spikes.gif)

Finally, [Google](https://developers.google.com/earth-engine/datasets) provides access to remote sensing data via its `earth engine` library. I will use this interface to perform zonal statistics on satellite images in the cloud before exporting tabular data.  

## questions

The question I am trying to answer is how much variation is there across "Downtown America" and what patterns occur regardless of the state or region? Which cities are similar and which is different? I will create a similarity score for urban cores, associating each city with others by the character its built environment, in order to relate cities to each other, along with clustering processes to group cities together.

## methods

I will scrape data on cities in the United States from [wikipedia](https://en.wikipedia.org/wiki/List_of_United_States_cities_by_population) and estimate the downtown by geocoding for "City Hall" in each city name—so a query would be "City Hall, City of New York, New York, USA". Most mapping and geocoding services will fail with such vague language (MapBox and OpenStreetMap struggled in tests), I will use Google Maps.

#### cities
![](https://raw.githubusercontent.com/asrenninger/wrangling/master/viz/context.png)

With coordinates for each city hall, I will use `osmnx` to select features within a given radius from them. With streets and buildings along with remote sensing data for each downtown (as I define it), I will then perform a series of calculations to assess the form.

This will include (though possibly more):
+ Fractal dimension
+ Network entropy
+ Mean building size
+ Node density
+ Mean edge length
+ Daytime/Nighttime population
+ Vegetation index

Finally, I will use clustering analysis to make sense of the relationships between cities. This will involve k-means by also hierarchical clustering—the former to understand broad groups and the latter to understand narrow dyads or triads. The similarity score will likely use a k-nearest neighbors distance calculation, borrowing from sports analytics, allowing us to make tables of first, second, and third most similar cities for any given city. Cities that are closest together are in a multidimensional space are more similar; cities farthest away are less similar.   

## Requirements

This project will use clustering and network analysis (requirement 5), will perform complex operations by using functional programming to aggregate data from many urban cores (requirement 4), and will require data from multiple sources, using not just `osmnx` but also population and vegetation data (requirement 3).
