
# Dashboards, Shiny Web Apps and Covid Data Analysis

Todays topics:

Recap of

- Data Processing
- Data Exploration
- Creating a new Project

Additionally Dashboards and Shiny Web App Development

## Topic Overview

Today we want to recap our whole processing pipeline in a small project
to find gaps in your knowledge and to give a small introduction into the
framework of `shiny`

As always: please install the following libraries:

``` r
# install from CRAN
install.packages("flexdashboard") # important package to realise dashboards
install.packages("shiny") # for the shiny web application
install.packages("zoo") # for the rolling mean method for averaging
```

![](data-science-program.png) \## Dashboards

Next to many more format ideas which are provided by R Markdown are
Dashboards. These are small HTML widgets which provide a nice overview
of data and plots. We will use the `flexdashboard` package. But there
are even more extensions and packages.

Please consult the file `Dashboard.Rmd` for a small example for the iris
dataset.

Usually Dashboards are for web services and should contain active
real-time elements. But they are also useful for presenting data for
upcoming users. Therefore This is just a very small showcase, what is
possible with R Markdown flexdashboards.

For showing big samples of dataframes or tibbles, it is recommended to
use the `knitr::kable` function to have a nicer output. Also one should
not print all values, but only a sample in case of Markdown documents or
PDF. In case of dynamic outputs like dashboards or pure HTML documents,
a scrollbar is inserted automatically.

``` r
library(tidyverse)
```

    ## ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
    ## ✔ dplyr     1.1.4     ✔ readr     2.1.4
    ## ✔ forcats   1.0.0     ✔ stringr   1.5.1
    ## ✔ ggplot2   3.4.4     ✔ tibble    3.2.1
    ## ✔ lubridate 1.9.3     ✔ tidyr     1.3.0
    ## ✔ purrr     1.0.2     
    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()
    ## ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors

``` r
iris %>% tibble -> data
data %>% sample_n(10) %>% knitr::kable()
```

| Sepal.Length | Sepal.Width | Petal.Length | Petal.Width | Species    |
|-------------:|------------:|-------------:|------------:|:-----------|
|          5.0 |         3.6 |          1.4 |         0.2 | setosa     |
|          5.4 |         3.4 |          1.5 |         0.4 | setosa     |
|          6.0 |         2.2 |          4.0 |         1.0 | versicolor |
|          5.1 |         3.8 |          1.9 |         0.4 | setosa     |
|          5.8 |         2.7 |          5.1 |         1.9 | virginica  |
|          7.2 |         3.0 |          5.8 |         1.6 | virginica  |
|          6.2 |         2.2 |          4.5 |         1.5 | versicolor |
|          6.3 |         2.5 |          5.0 |         1.9 | virginica  |
|          4.7 |         3.2 |          1.3 |         0.2 | setosa     |
|          6.5 |         3.0 |          5.8 |         2.2 | virginica  |

The icons which are used in the dashboard, are based on the icons from
[Font Awesome](https://fontawesome.com/). You can pick them there and
use them in the dashboard.

Usually Dashboards are used in combination with the `Shiny` package.
This allows to publish and use the dashboard through a whole web
application.

**Exercise:** Construct a basic dashboard about WDI values with 1 page
and 3 panes. Choose an index and show a world map with projected index
values in a specific year. In a second pane plot the time series for a
specific country. Finally, describe in a few lines, what is shown in the
plots in the third pane.

## Covid-19 Data

Today’s data is about Covid-19 and comes from the organization “Our
World in Data”. Either download the data on your own
[here](https://github.com/owid) or simply download it from Moodle. The
exact link is
`https://covid.ourworldindata.org/data/owid-covid-data.csv`. It is also
possible to read the data directly from the internet through `read_csv`
from the `tidyverse` packages.

**Exercise**:

1.  Your first task is to load and explore the dataset a little bit
2.  Create a map to show the amount of total cases in each country on
    January 1st, 2023
3.  Also create a new tibble with only German data of new cases,
    smoothed new cases and total cases. Look at some plots.
4.  Repeat it but remove all `na` values before from the data
5.  Use the following mutation to create an averaged mean:
    `%>% mutate(new_cases_avg = rollmean(new_cases, k=5, fill=NA, align='right')) %>% drop_na(new_cases_avg)`.
    You need to load the `zoo` package before with `library(zoo)`.
6.  Create a plot to inspect the curve for new cases in Germany. How
    does the parameter `k` control the output?
7.  Add nice axes descriptions to the plot and add a title

## Shiny web app

Shiny is a small framework to create a dashboard like `flexdashboard`.
There is an unfinished example in the folder `ShinyCovid`. Copy the file
`owid-covid-data.csv` in a new folder with the name `data` inside
`ShinyCovid`. Then run the application with `runApp("ShinyCovid/")`
after loading the `shiny` package.

**Exercise**:

1.  The new framework creates a small application, which is reactive.
    Play around with it a bit
2.  Look at the source of the file `App.R`. The shiny part is divided
    into three parts: setup if the ui, setup of the server and running
    both together. Find out with the source code, how to access the
    input values.
3.  Rewrite the style of the app a little bit to make it a bit nicer
    (less headings, a short sentence to introduce the data)
4.  Add a title and labels to the plot
5.  remove all `na` values before plotting from the data
6.  Change the data which is shown to the smoothed version, which was
    introduced from the `zoo` package. Don’t forget to load the package
    in the `app.R` file at the proper location. Use a fixed `k=5`.
7.  Access the radio button and include an `if` clause in your
8.  Access the slider variable to control the parameter `k`.
9.  Which `k` was probably used to smooth the Covid data?
