Baltimore Passive Income Households
```
#get_acs is used to call # of households reporting passive income and # of households in each tract. The call also specifies to include geometry and cartographic boundaries.
interest_vars <- c(
  households = "B11012_001",
  passive_hh = "B19054_002"
)
interest <- get_acs(
  geography = "tract", 
  variables = interest_vars, 
  state = "MD", 
  county = "Baltimore city",
  output = "wide",
  geometry = TRUE,
  cb = TRUE,
  year = 2019
)

#creates a new dataset with a new field using mutate
#phh_per_hh contains the number of households that report income from interest, dividends, or rental properties per 100 households in its respective tract 
interest_per <- interest %>%
  mutate(phh_per_hh = (100*(passive_hhE/householdsE)))
  ```
  ---
  ```
  #used ggplot to plot the phh_per_hh field.
#themedubois is used to style the plot
#extrafont is used to load the Jefferies font from windows
#ggtitle is used to add a title
#labs is used to add a legend title and caption
#scale_fill_distiller is used to specify scale color and reverse it
#theme is used to format title and caption
ggplot(interest_per)+
  geom_sf(aes(fill = phh_per_hh))+
  theme_dubois()+
  ggtitle("Where are the Baltimore \nLand- and Crypto- Lords?")+
  labs(fill = "% Passive Income Households", caption = "Households Reporting 'Interest, Dividends, Or Net Rental Income.' \nData from 2019 American Community Survey.") +
  scale_fill_distiller(palette = "YlOrRd", direction = 1)+
  theme(plot.caption.position = "plot",
        plot.caption = element_text(hjust = 0,
                                    size = 8))
                                    
                                    
```
