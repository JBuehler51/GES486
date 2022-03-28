Maryland Public Transit
```
#used get_acs to call data on percent of residents who use public transit to commute. Also called a population variable as a reference.
#used map and map2 in order to return a dataset with the desired variables as well as geometry over the period 2010 to 2019.
years <- 2010:2019

transit1019 <- map(
  years,
  ~ get_acs(
    geography = "county",
    variables = "DP03_0021P",
    summary_var = "B01003_001",
    geometry = TRUE,
    cb = FALSE,
    year = .x,
    state = "MD"
    
    
    )
  ) %>% 
  map2(years, ~ mutate(.x, id = .y))

MD2 <- reduce(transit1019, rbind)
#cb = FALSE should remove water, but does not in this case
```
---
```
MD_raw <- MD2 %>% 
  mutate(total_pt_com = (estimate/100)*summary_est)
#calculates a raw total of the population who use public transit
```
---
```
#referenced this page for rates of change calculation. https://stackoverflow.com/questions/48533631/calculating-rate-of-change
#used group_by and arrange data by year and county
#mutate is used to add a field with the rate of change output for each year and county
MD_rates <- MD_raw %>% 
  group_by(NAME) %>% 
  arrange(NAME, id) %>% 
  mutate(rate = 100 * (total_pt_com - lag(total_pt_com))/lag(total_pt_com)) %>%
  ungroup()
```
  ---
```
#this is a mess
#used filter to create individual datasets for each year interval. exp. MD_2011 includes rates for every county during the period 2010-2011.
MD_2011 <- MD_rates %>%
  filter(id == 2011)
MD_2012 <- MD_rates %>%
  filter(id == 2012)
MD_2013 <- MD_rates %>%
  filter(id == 2013)
MD_2014 <- MD_rates %>%
  filter(id == 2014)
MD_2015 <- MD_rates %>%
  filter(id == 2015)
MD_2016 <- MD_rates %>%
  filter(id == 2016)
MD_2017 <- MD_rates %>%
  filter(id == 2017)
MD_2018 <- MD_rates %>%
  filter(id == 2018)
MD_2019 <- MD_rates %>%
  filter(id == 2019)
```
---
```
#used this chunk to output a plot for each dataset created above
#used ggplot to visualize each counties rate of change
#theme_void removes lat long lines
#scale_fll_gradient used to specify a diverging scale and add value limits
#ggtitle used to add a title
#labs used to add a legend title, caption and subtitle
#theme used to format title, caption and subtitle
ggplot(MD_2019)+
  geom_sf(aes(fill = rate))+
  theme_void()+
  scale_fill_gradient2(low = "red", mid = "white", high = "blue", limits = c(-140,140))+
  ggtitle("Yearly Rates of Change for Maryland Public Transit Usage")+
  labs(fill = "Rate", caption = "2018-2019", subtitle = "Data from American Community Survery")+
  theme(plot.caption = element_text(hjust = 1,
                                    size = 12),
        plot.title = element_text(size = 14),
        plot.subtitle = element_text(size = 10))
 ```
