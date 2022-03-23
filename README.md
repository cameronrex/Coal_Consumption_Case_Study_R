# Coal_Consumption_Case_Study_R
Data Wrangling Case Study in R using Coal Consumption Data

#From LinkedIn Learning-Data Wrangling in R (2017)

### Load the tidyverse
library(tidyverse)

### Read in the coal dataset
coal <- read_csv("http://594442.youcanlearnit.net/coal.csv")
glimpse(coal)

### Skip the first two lines
coal <- read_csv("http://594442.youcanlearnit.net/coal.csv", skip=2)
glimpse (coal)

### Rename the first column as region
colnames(coal)[1] <- "region"
summary(coal)

### Convert from a wide dataset to a long dataset using gather
coal_long <- gather(coal, 'year', 'coal_consumption', -region)
glimpse(coal_long)

### Convert years to integers
coal_long$year <- as.integer(coal_long$year)
summary(coal_long)

### Convert coal consumption to numeric
coal_long$coal_consumption <- as.numeric(coal_long$coal_consumption)
summary(coal_long)

### Check region values-they have both continents and countries
unique(coal_long$region)

### Create a vector of noncountry values that appear in the region variable
noncountries <- c("North America", "Central & South America", "Europe", "Eurasia", "Middle East", "Africa", "Asia & Oceania", "World")

### Look for matches
matches <- which(!is.na(match(coal_long$region, noncountries)))

### Create a tibble for country and region values
coal_country <- coal_long[-matches, ]
coal_region <- coal_long[matches, ]

### Review the vales
unique(coal_country$region)
unique(coal_region$region)

### Plot the coal_consumption data by region
ggplot(data=coal_region, mapping=aes(x=year, y=coal_consumption)) +
  geom_line(mapping=aes(color=region))
