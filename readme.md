(Reproducible) Data Visualisation with R and how to make interactive
things with R
================

This repo contains my 2022-07-13 talk for the [Artificial Intelligence
for Scientific Discovery Network+
(AI3SD)](https://www.ai4science.network/ai3sd-event/13-14-07-2022-failed-it-to-nailed-it-nailing-your-data-visualisation-hands-on-workshop/)
workshop *Nailing your Data Visualisation: Training Workshop*.

Please note that this talk is approximately 50% live coding so the
slides won’t make too much sense on their own. [You can download the
code I wrote during the
workshop](https://drive.google.com/file/d/1QbZR2Bca2MYdbynKCebq85LTblXAKv2W/view?usp=sharing).
[A PDF copy of the talk is available
here](https://github.com/charliejhadley/talk_2022-07-13_ai3d-interactive-dataviz-with-r/raw/main/all-slides-pdf.pdf).

During the talk I made use of this etherpad,
[pad.riseup.net/p/2022-07-13_cjh](https://pad.riseup.net/p/2022-07-13_cjh).
I’ve stored the contents of the etherpad in the `talk-etherpad.md` file.

## Required packages

To follow along with the live coding you will need to install several
packages:

``` r
install.packages(c("fivethirtyeight", "fontawesome", "ggplot2", 
  "here", "highcharter", "janitor", "leaflet", "plotly", "rmarkdown", "rnaturalearthdata", 
  "sf", "shiny", "tidyverse", "xaringan", "visNetwork"))
```
