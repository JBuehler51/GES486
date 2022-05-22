My goal for this final project is to see if distribution of farmable land correlates to income in Montgomery County, MD. I thought it would be interesting to see if communities that would benefit the most from local food production even had the land to support it. I had the idea to answer this question after browsing USGS soil and water data. This led me to find that the National Resource Conservation Service (NRCS) classifies soil as being “Prime Farmland”, “Farmland of Statewide Importance”, and “Not Prime Farmland.” The NRCS defines prime farmland to have the “best combination of physical and chemical characteristics for producing food” (https://www.nrcs.usda.gov/Internet/FSE_DOCUMENTS/nrcseprd1338623.html). I intentionally left out land designated “Farmland of Statewide Importance” because they explicitly do not meet the NRCS physical standards to be prime farmland. 

<img src="/images/Soil.Class.jpg?raw=true">

To complete this analysis, I will be calculating the total area of prime land within each Montgomery County Census tract. Then, to normalize those areas, I divide prime area by total usable tract area. I define usable tract area as any area that is not currently taken up by water or a building footprint (I could have included roads/parking lots as well). The shapefiles for water and building footprints came from Montgomery County’s data portal.

<img src="/images/MoCo_Prime.jpg?raw=true">

I then went on to do areal interpolation for distribution of prime farmland and median income 

<img src = "/images/Areal.Prime.jpg?raw=true">

<img src="/images/Areal.Inc.png?raw=true">

[R Markdown File](/pdf/Soil_Data5.pdf)
