Gathering data
```
# this chunk gathers data from the American Community Survey for a number of variables for the year 2019.
variables_to_get <- c(
  median_value = "B25077_001",
  median_income = "DP03_0062",
  total_population = "B01003_001",
  median_age = "B01002_001",
  pct_college = "DP02_0068P",
  pct_foreign_born = "DP02_0094P",
  pct_white = "DP05_0077P",
  median_year_built = "B25037_001",
  percent_ooh = "DP04_0046P"
)

BC_tracts <- get_acs(
  geography = "tract",
  variables = variables_to_get,
  state = "MD",
  county = "BAltimore City",
  geometry = TRUE,
  output = "wide",
  year = 2019
)
#The "Count Points in Polygons" tool in QGIS was used to add a count of trees in tracts of the BC_tracts file
#BC_tracts was exported from RStudio using the st_write function
#a tree point shapefile was obtained externally
#BC_tracts with counted trees was brough back into RStudio with the code below
BC_Trees2 <- st_read("C:/Users/buehl/Documents/GES 486/Lab 4/Data/Shapefiles/BC_tracts_trees.shp")
```
---
Spatial Analysis
```
# poly2nb() and nb2listw() are used to create a spatial weight matrix
BCb2 <- poly2nb(BC_Trees2, queen = T)
BCw2 <-nb2listw(BCb2, style="W", zero.policy = TRUE)

#employ spatial lag model using list created above
fit.lagBC2<-lagsarlm(Count_Tree ~ mdn_vlE + mdn_ncE + pct_whE,  data = BC_Trees2, listw = BCw2) 

#inspect lag model
summary(fit.lagBC2)
```
---
Plotting with ggplot and patchwork
```
#scipen eliminates scientific notation in plots
options(scipen=999)
#creates a map of madian housing prices in Baltimore City
#scale::dollar adds $ labels to the legend
gg1 <- ggplot(BC_Trees2)+
  geom_sf(aes(fill = mdn_vlE)) +
  scale_fill_distiller(palette = "YlOrRd", direction = 1, labels = scales::dollar)+
  labs(title = "Median Housing Prices",
       fill = "Median Value")+
  theme_void()
#creates a map of tree counts for Baltimore City
gg2 <- ggplot(BC_Trees2)+
  geom_sf(aes(fill = Count_Tree))+
  scale_fill_distiller(palette = "YlOrRd", direction = 1)+
  labs(title = "Number of Trees per Census Tract", 
       fill = "Trees per Tract")+
  theme_void()
#creates a scatter plot showing Trees per Tract and Median Values
#scale_y_continuous is used to edit the y axis lables 
gg3 <- ggplot(BC_Trees2)+
  geom_point(aes( x = Count_Tree, y = mdn_vlE))+
  labs(title = "Trees per Census Tract Compared
       to Median House Price",
       y = "Median Value ($)",
       x = "Trees per Tract")+
  scale_y_continuous(labels = function(x) paste0(abs(x / 1000), "k"))
#handy patchwork package used to arrange each plot, add a title, and caption. 
patchwork <- (gg1 / gg2) | gg3
patchwork + plot_annotation(
  title = 'Do Trees Influence Housing Prices in Baltiore City?',
  caption = '2019 American Community Survey'
)
````
