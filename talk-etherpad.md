# Packages you need


install.packages(c("fivethirtyeight", "fontawesome", "ggplot2",
  "here", "highcharter", "janitor", "leaflet", "plotly", "rmarkdown", "rnaturalearthdata",
  "sf", "shiny", "tidyverse", "xaringan", "visNetwork"))


# Repo for my slides

https://github.com/charliejhadley/talk_2022-07-13_ai3d-interactive-dataviz-with-r

# Code I wrote during talk

https://drive.google.com/file/d/1QbZR2Bca2MYdbynKCebq85LTblXAKv2W/view?usp=sharing

=======

long_data_tidy <- long_data %>%
  mutate(coord = "longitude") %>%
  mutate(dive_position = row_number()) %>%
  pivot_longer(starts_with("X"))

lat_data <- tribble(
  ~X1, ~X2, ~X3,
  -10, -10, 11,
  -22, 32, 22
)

lat_data_tidy <- lat_data %>%
  mutate(coord = "latitude") %>%
  mutate(dive_position = row_number()) %>%
  pivot_longer(starts_with("X"))

long_data_tidy %>%
  bind_rows(lat_data_tidy)

=============== XXXX FULLY WRITTEN SOLUTION XXXX ========================

library(tidyverse)
library(leaflet)
library(leafpop)
library(leaflet.extras)

long_data <- tribble(
  ~X1, ~X2, ~X3,
  -57.3, -47, 37,
  -40, 30, 20
)

long_data_tidy <- long_data %>%
  mutate(coord = "longitude") %>%
  mutate(dive_position = row_number()) %>%
  pivot_longer(starts_with("X"))

lat_data <- tribble(
  ~X1, ~X2, ~X3,
  -10, -20, 31,
  -22, 32, 22
)

lat_data_tidy <- lat_data %>%
  mutate(coord = "latitude") %>%
  mutate(dive_position = row_number()) %>%
  pivot_longer(starts_with("X"))

location_data <- long_data_tidy %>%
  bind_rows(lat_data_tidy) %>%
  pivot_wider(values_from = value,
              names_from = coord) %>%
  arrange(dive_position, name)


temp_data <- tribble(
  ~X1, ~X2, ~X3,
  300, 299, 310,
  200, 180, 170
)

temp_data_tidy <- temp_data %>%
  mutate(dive_position = row_number()) %>%
  pivot_longer(starts_with("X"),
               values_to = "temperature")


dive_data <- location_data %>%
  left_join(temp_data_tidy)


# temperature chart -------------------------------------------------------
# Assume chart same for all dive_position == 1

# Make just the one chart
temp_data_tidy %>%
  filter(dive_position == 1) %>%
  ggplot(aes(x = name,
             y = temperature,
             group = dive_position)) +
  geom_line()

# Turn into a function

make_dive_temp_chart <- function(dive_number){

  dive_data %>%
    filter(dive_position == 1) %>%
    ggplot(aes(x = name,
               y = temperature,
               group = dive_position)) +
    geom_line() +
    labs(title = paste("Data for dive number", dive_number))

}

 make_dive_temp_chart(3)


# Programmatically generate charts ----------------------------------------
# We need to programmatically generate charts for all location points in
# the dataset


list_gg_temp_charts <- dive_data %>%
  pull(dive_position) %>%
  map(~make_dive_temp_chart(.x))



lf_dive_map <- leaflet() %>%
  addProviderTiles(providers$Esri.OceanBasemap) %>%
  addCircleMarkers(data = location_data,
                   weight = 1,
                   color = "black",
                   fillColor = "purple",
                   fillOpacity = 1,
                   group = "dives",
                   radius = 4,
                   label = ~str_glue("Dive number {dive_position}, position {name}")) %>%
  addPopupGraphs(list_gg_temp_charts,
                 group = "dives",
                 width = 600, height = 300)

lf_dive_map
