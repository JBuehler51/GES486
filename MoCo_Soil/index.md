[R Markdown File](/pdf/Soil_Data5.pdf)

My goal for this final project is to see if distribution of farmable land correlates to income in Montgomery County, MD. I thought it would be interesting to see if communities that would benefit the most from local food production even had the land to support it. I had the idea to answer this question after browsing USGS soil and water data. This led me to find that the National Resource Conservation Service (NRCS) classifies soil as being “Prime Farmland”, “Farmland of Statewide Importance”, and “Not Prime Farmland.” The NRCS defines prime farmland to have the “best combination of physical and chemical characteristics for producing food” (https://www.nrcs.usda.gov/Internet/FSE_DOCUMENTS/nrcseprd1338623.html). I intentionally left out land designated “Farmland of Statewide Importance” because they explicitly do not meet the NRCS physical standards to be prime farmland. 

<img src="/GES486/images/Soil.Class.jpg?raw=true">

To complete this analysis, I will be calculating the total area of prime land within each Montgomery County Census tract. Then, to normalize those areas, I divide prime area by total usable tract area. I define usable tract area as any area that is not currently taken up by water or a building footprint (I could have included roads/parking lots as well). The shapefiles for water and building footprints came from Montgomery County’s data portal.

<img src="/GES486/images/MoCo_Prime.jpg?raw=true">

I then went on to do areal interpolation for distribution of prime farmland and median income 

<img src = "/GES486/images/Areal.Prime.jpg?raw=true">

<img src="/GES486/images/Areal.Inc.png?raw=true">

Looking at these two plots, there are some similarities in their distribution. The distribution pattern of high income and high proportions of prime farmland is similar in the central region. They really deviate in the southern section’s neighboring DC where median income soars and prime farmland is rare. The next step to confirm any connection between the two variables would be to build a regression curve. To do this I used the corrr package in R which uses the Pearson correlation method. The results were unfortunate.

<img src="/GES486/images/Corr_Plot.jpg?raw=true">

We can see that most of the Census variables correlate with one another. For example, tracts with higher median income also have homes with a greater number of rooms. Unfortunately, “Prime_to_Area1,” the proportion I calculated for prime farmland, did not correlate to any of the variables I collected. 
