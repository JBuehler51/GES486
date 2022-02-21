## Plot 1
This plot was made using the tidycensus package within R to call data from the single year American Community Survey. 
```
metromd <-  get_acs(
  geography = "county",
  variables = "DP03_0021P",
  summary_var = "B01003_001",
  state = "MD",
  survey = "acs1",
  year = 2019
)
```
String remove function was used to edit the returned county names. Data is recorded by the Census as a percent. Ggplot was used to visuallize the data.
```
metromd %>%
   mutate(NAME = str_remove(NAME, ",.*$")) %>%
  ggplot(aes(y = (reorder(NAME,estimate)), x = estimate)) + 
  geom_col(color = "red", fill = "red") +  
  theme_minimal(base_size = 12, base_family = "Verdana") +
  scale_x_continuous(labels = ~paste0(.x, "%"))+
  geom_errorbarh(aes(xmin = estimate - moe, xmax = estimate + moe))+
  labs(title="Maryalnd Public Transit Usage by County", 
       subtitle="2019, 1 year ACS",
       y="Counties",
       x="ACS estimate")
```
