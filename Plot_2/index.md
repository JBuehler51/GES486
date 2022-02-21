## Plot 2
This plot uses the tidycensus package and map_dfr function to call data on median hosuing prices by state over the years 2005 to 2019 from the American Communiuty Survey.
```
years <- 2005:2019
names(years) <- years

Median_values <- map_dfr(years, ~{
  get_acs(
    geography = "state",
    variables = "B25077_001",
    year = .x,
    survey = "acs1"
  )
}, .id = "year")
```
The data was visualized using the ggplot2 and geofacet packages. Paste0 and breaks function was used to edit the axis labels.
```
options(scipen = 999)
ggplot(Median_values, aes(x = year, y = estimate, group = NAME)) +
  geom_line() +
  facet_geo(~ NAME, grid = "us_state_grid2", label = "name") +
  scale_x_discrete(breaks = c(2008, 2019)) +
  scale_y_continuous(labels = function(x) paste0(abs(x / 1000), "k")) +
  labs(title = "Median Housing Prices from 2005-2019",
    caption = "Data From American Community Survey",
    x = "Year",
    y = "Median Price ($)") +
  theme(axis.text = element_text(size= 5), strip.text.x = element_text(size = 5))
  ```
