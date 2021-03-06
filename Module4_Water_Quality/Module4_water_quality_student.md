Module 4: Water Quality
================
FREC 3004: Environmental Informatics

``` r
library(tidyverse)
library(lubridate)
```

## Science question

How often do rivers in the U.S. exceed the EPA limit for water quality?

## Environmental Learning Objectives:

  - Understand the variability of concentrations of dissolved substances
    in stream water and identify some reasons for this variability.
  - Find regulatory guidelines for nitrate concentrations in surface
    water, find real-time nitrate concentration data, and recognize when
    observed concentrations exceed guidelines
  - Evaluate natural and anthropogenic factors that cause the
    concentrations of nitrate to change over time
  - Define the meaning of “statistical probability,” and use this term
    as a regulatory tool to evaluate a potential drinking water resource

## Data Science Learning Objectives

  - Practice importing data sets that aren’t in a standard header format
  - Apply if-statements and ifelse()()
  - Apply loops for scaling-up analyses
  - Pasting strings and numbers together
  - Communicating with an API

## Assumed R knowledge

  - mutate()
  - group\_by()
  - ggplot()
  - geom\_line()
  - geom\_point()
  - select()
  - summarize()
  - mean()
  - filter()
  - rename()
  - read\_csv()
  - lm()
  - rbind()
  - geom\_abline()
  - geom\_smooth()
  - geom\_histogram()
  - head()

## R knowledge covered in module

  - read\_delim()
  - rename()
  - slice()
  - as.numeric()
  - paste0()
  - for loop
  - if-else statement
  - ifelse()
  - vector()
  - geom\_hline()

## Why this matters

Water is one of the earth’s most precious resources, and is involved in
almost many biogeochemical processes. Human activities often lead to
problems with water quality. Concentrations of dissolved substances in
freshwater varies over time and space, and depends on surrounding
anthropogenic factors. Water quality significantly impacts not only
ecosystem health (plants, animals, microorganisms, and other species)
but also humans who depend on freshwater resources for recreation,
tourism, and our very basic needs of waste management and drinking
water. This module aims to help students understand the variability of
concentrations of dissolved substances in stream water and identify some
reasons for this variability. You will explore and evaluate water
quality concerns with real-time data from regulatory entities, and learn
about water quality ecosystem and human health implications.

## Outline:

1)  Explore impaired streams and real-time nitrate concentrations
2)  Download data
3)  Import data
4)  Visualize the data
5)  Analyze the data
6)  Scaling up to multiple sites
7)  Analyzing new state
8)  Analyzing multiple time periods

## Step 1: Explore impaired streams and real-time nitrate concentrations

For the second part of this module, we will be further exploring
impaired water bodies near us and federal guidelines for safety using
publicly available EPA water quality data.

**Question 1:** (provide answers)

1.  Explore federal regulatory guidelines. The US Environmental
    Protection Agency lists water quality regulations for both Human
    Drinking Water
    <https://www.epa.gov/ground-water-and-drinking-water/national-primary-drinking-water-regulations>

<!-- end list -->

1.  What is the Maximum Contaminant Level (MCL) for nitrate (reported as
    nitrate -nitrogen) in mg/L.

<!-- end list -->

``` r
EPA_limit <- 
```

2.  What are the potential health impacts of consuming water with
    concentrations above this limit?

3.  What are the common sources of nitrate in water?

## Step 2: Download data

Collect, plot, and explain the water quality data to do an assessment of
acute, local, water quality impacts.

During this portion of the module, you will search for and work with
local water quality data, and generate plots that will allow you to
further analyze the data, answering questions as you go.

Watershed managers need to know how often a risk presents itself in a
watershed in order for action to be taken. For financially strapped
government’s, priority is given to problems that have the highest
probability of occurring and which are associated with the most severe
impacts. “Blue-baby syndrome” is a condition that leads to infant
mortality. It is caused by the ingestion of nitrate in drinking water
which subsequently bonds to oxygen sites on hemoglobin in the blood of
the infant. This impairs the circulation of oxygen in the bloodstream
and causes the baby to turn blue. Obviously, society would like to avoid
this outcome.

Let’s assume that the Cedar River, Iowa, is being considered for use as
a drinking water supply by the municipality of Palo, Iowa (note, this is
probably not happening in reality). Is this river water safe for human
consumption? To answer this question, we will first explore the
variability of nitrate within this river.

  - Open the U.S. Geological Survey’s “Water Quality Watch” web page:
    <http://waterwatch.usgs.gov/wqwatch/>
  - Under the “State” menu, select “Iowa” and allow the map to refresh.
  - Under the measurement menu, select “Nitrate.”
  - Move your mouse over the available triangles until you find “Cedar
    River at Blairs Ferry Rd, at Palo, IA.” This triangle is equidistant
    from the top (North) and the bottom (South) of the map, and about an
    inch to the left of the right (East) boundary of the state.
  - Once you find it, click once on the triangle, and the click again on
    the eight-digit hydrologic unit code (HUC) number “05464420” which
    identifies this site.
  - Now you are in the official USGS data site for the Cedar River at
    Blairs Ferry Rd, Palo, IA. Scroll down and explore the plots of the
    available data. We would like to see whether nitrate concentrations
    change over the course of the agricultural season.  
  - Scroll back up to the box labelled “Available Parameters.”
  - Uncheck all except for “NO3+NO2,water,insitu”
  - Change the starting date to April 1, 2014 and the end date to
    September 1, 2014. Enter these dates in YEAR-MO-DY format
    (i.e. 2014-04-01, and 2014-09-01, respectively).
  - Under the heading “Output Format,” select the “Tab-separated” circle
    and click “GO.”
  - Copy the data into a text file (in RStudio: File -\> New file -\>
    Text File) and save as `site_05464420.txt` into your data directory
    for this module (you should have creating a Module 4 directory with
    different sub directories for data, etc. and a R project for the
    Module).

## Step 3: Import data

Try reading in the data with the `read_csv()` function

``` r
#INSERT CODE
```

Did it work? Now open the file in a text editor. How is this file
different that other files we have worked with? What are the header
rows?

Try a more general function (`read_delim()`) for import files that are
not comma delimitated. " for the delim tells the function to look for a
tab to separate the cells.

``` r
stream_data <- read_delim("data_raw/site_05464420.txt", delim = "\t")
```

Look at the imported data. Did it work? Look at the file again in a text
editor (i.e., In RStudio you can open the file, file -\> open file).  
What is line number has the header row? Is there a symbol that
designates a comment in the file?

Now use the comment option in the read\_delim. You will need to add the
comment symbol below in place of \[INSERT SYMBOL\]

``` r
stream_data <- read_delim("data_raw/site_05464420.txt", 
                          delim = "\t", 
                          comment = "[INSERT SYMBOL]")
```

Do the headers look good now?

``` r
head(stream_data)
```

Which column is the nitrate observation? Let’s give them better names.
The number below in `rename` is the column number

``` r
stream_data <- stream_data %>% 
    rename("nitrate" = 5)
```

What is the class of the datetime and nitrate columns?

Is this what you expected?

Why might the class be wrong? Look at the top rows of the dataframe and
in the raw data. Why is the first row different that all the other rows?

Now, we need to remove the first row

There are two ways:

1)  `stream_data <- stream_data[-1, ]` is the way the we learned in the
    data carpentry module. This removes the first row

or

2)  A way that works with tidyverse and pipes. `slice` is a function
    that selects particular rows.

<!-- end list -->

``` r
stream_data <- stream_data %>% 
    slice(-1)

head(stream_data)
```

Now fix the class issue with the `datetime` and `nitrate` columns. What
is the format of the datatime? Since it has the year, month, day, hour,
and minute we will use the `ymd_hm()` function (this is similar to the
`ymd()` that we used in the data carpentry module)

``` r
stream_data <- stream_data %>% 
    mutate(datetime = ymd_hm(datetime))
head(stream_data)
```

Now lets fix the `nitrate` column by converting from character to
numeric using `as.numeric()`

``` r
#INSERT CODE
```

Finally, remove the nitrate missing values

``` r
#INSERT CODE
```

Now combine all data cleaning steps into a single piped command

``` r
stream_data <- read_delim("data/site_05464420.txt", 
                          delim = "\t", 
                          comment = "[INSERT SYMBOL]")
stream_data <- stream_data %>% 
  #[INSERT STEPS FROM ABOVE]

head(stream_data)
```

## Step 4: Visualize the data

Create plot with datetime on the x-axis and nitrate on the y-axis (this
is a time-series plot of nitrate). Then use the `geom_hline()` function
to add a horizontal line at the EPA concentration (the `EPA_limit`
variable that you defined above)

``` r
#INSERT PLOT
```

**Question 2:** Does the location exceed the EPA limit?

**Answer 2:**

## Step 5: Analyze the data

Calculate the percentage of time that the stream exceeds the EPA limit.

Hints: you will use the `ifelse()`, `sum()`, `n()`, `mutate()`, and
`summarize()` functions. The `ifelse()` function is likely new to you.
You use the ifelse() function within the mutate function. For example
following code will set the value of `new_variable` equal to 1 IF
’old\_variable`is greater than 1, otherwise (i.e., ELSE) set the value
of`new\_variable\` equal to 0.

`mutate(new_variable = ifelse(old_variable > 1, 1, 0))`

Be sure you print out your results for calculating the percentage of
time that the steam exceeds the EPA limit so that it shows up in the
knitted document.

``` r
exceed <- stream_data %>% 
  #INSERT CALCULATIONS TO DETERMINE THE PERCENTAGE OF OBSERVATIONS
  #THAT EXCEED THE EPA LIMIT

exceed
```

## Step 6: Scaling up to multiple sites

Now we want to calculate the probability of exceeding the EPA limit for
all gauged streams and river in Iowa. It would be very slow to manual
click and download files for all gauges and time points that we are
interested in.  
Fortunately there is a way around this that is more advanced (but hey
you are studying environmental informatics). The process of automating
the download and calculations requires a few new concepts.

First, many sources of data have an application programming interface
(API) that is a set of clearly defined methods of communication. The
USGS has a API that will use.

Copy and paste the following command into a web
browser.

<https://nwis.waterdata.usgs.gov/usa/nwis/uv/?cb_99133=on&format=rdb&site_no=05464420&period=&begin_date=2014-04-01&end_date=2014-09-01>

See how this produces the same file as what you downloaded manually.

Now look at the web address. The format of the web address is the AP.
There is “\&site\_no”, “\&begin\_date”, “\&end\_date” in the address. We
can modify these to download other sites and other time periods.

Create a variable called site and set it equal to our focal site
number.  
Similarly, create variables for the begin and end date

``` r
site <- 
begin_date <- 
end_date <- 
```

Now use the `paste0()` function combine the components of address with
the variables to create a new web address where we can easily change the
address with new sites and time
periods

``` r
url <- paste0("https://nwis.waterdata.usgs.gov/usa/nwis/uv/?cb_99133=on&format=rdb&site_no=", 
              site, 
              "&period=&begin_date=",
              begin_date, 
              "&end_date=", 
              end_date)
url
```

Now download the file at the web address using R

``` r
download.file(url = url,
              dest = paste0("data_raw/site_", site, ".txt"))
```

We are now one step closer to automating.

Second, we need to be able to loop through a vector of different sites
and download each of those sites. We will use the concept of a “for
loop”.

Create a vector of sites. The following was obtained from the USGS
website and represents all of the active sites in Iowa

``` r
site <- c("05412500",
          "05418400",
          "05418720",
          "05420500",
          "05455100",
          "05465500",
          "05464420",
          "05481000",
          "05482000",
          "05482300",
          "05482500",
          "05483600", 
          "05484000", 
          "05484500",
          "06808500", 
          "06817000")
```

A “for loop” will increment through a vector. The following example
loops through the values 1 through 10 and names the current value of the
loop. It prints the current value of i.

``` r
for (i in 1:10) {
  print(i)
}
```

We can use this current value of i to subset the site vector. Note that
we change the highest value in the loop from 10 to the length of the
site vector

``` r
for (i in 1:length(site)) {
  print(site[i])
}
```

Now we can use the subsetted site vector to modify the site value in the
url.  
See that we replaced site with site\[i\]

``` r
for (i in 1:length(site)) {
  url <- paste0("https://nwis.waterdata.usgs.gov/usa/nwis/uv/?cb_99133=on&format=rdb&site_no=", 
                site[i], 
                "&period=&begin_date=", 
                begin_date, 
                "&end_date=", 
                end_date)
  print(url)
}
```

``` r
url <- vector("character", length = length(site))
for (i in 1:length(site)) {
  url[i] <- paste0("https://nwis.waterdata.usgs.gov/usa/nwis/uv/?cb_99133=on&format=rdb&site_no=", 
                site[i], 
                "&period=&begin_date=", 
                begin_date, 
                "&end_date=", 
                end_date)
}
print(url)
```

Third, combine your code from above that you used to calculate the
probability of exceeding the EPA threshold, the API example, and the
“for loop”" to calculate the probability of exceeding the EPA
threshold for all the streams and rivers in Iowa.

``` r
#Vector of site numbers
site <- c("05412500",
          "05418400",
          "05418720",
          "05420500",
          "05455100",
          "05465500",
          "05464420",
          "05481000",
          "05482000", 
          "05482300", 
          "05482500", 
          "05483600", 
          "05484000", 
          "05484500",
          "06808500", 
          "06817000") 

#Create a vector to hold the calculation for each gauge
site_exceed <- vector("double", length = length(site)) 

EPA_limit <-

for (i in 1:length(site)) {
  
    download.file(url = paste0("https://nwis.waterdata.usgs.gov/usa/nwis/uv/?cb_99133=on&format=rdb&site_no=", 
                               site[i], 
                               "&period=&begin_date=", 
                               begin_date, 
                               "&end_date=", 
                               end_date),
                dest = paste0("data_raw/", site[i], ".txt"))

  stream_data <- read_delim(paste0("data_raw/", site[i], ".txt"), 
                            delim = "\t", 
                            comment = "#")
  
  if (ncol(stream_data) > 5) { #THIS IS NEED TO SKIP OVER POTENTIALLY BAD DATA FILES
  
    #INSERT CODE FROM ABOVE TO TIDY UP DATA AND CALCULATE THE PROBABILITY OF EXCEEDING THE EPA THRESHOLD
    
   
    
    site_exceed[i] <- #YOUR CALCULATION FOR THE SITE
  }else{
    site_exceed[i] <- NA
  }
  
}

#combine the site ID and exceed columns into a data frame (tibble)
site_exceed_combined <- tibble(site = site, 
                               exceed = site_exceed)
print(site_exceed_combined)
```

Fourth, make a bar graph showing the probability of exceeding the EPA
threshold for each station in your analysis.

In the geom\_bar(), you will use `stat = "identity` (which say “just use
the y variable that I provide”)

Be sure your station numbers are readable on the plot.

``` r
#INSERT CODE
```

## Step 6: Analyzing a new state

**Question 3** Answer the following question by reusing and modifying
the code above: What was the probability of exceeding the EPA threshold
for all gauges in Virginia between 2014-04-01 and 2014-09-01?

You will get the list of Virginia gauges by 1) Going to
<https://waterwatch.usgs.gov/> 2) Selecting Virginia and Nitrate from
the drop-down options 3) Clicking “Site List”

The numbers in front of each station are the numbers used in the API. Be
sure your answer prints out to the document when you knit it.

**Answer 3:**

``` r
#INSERT CODE
print(site_exceed)
```

**Question 4:** How does the probability of exceeding the EPA threshold
differ between Iowa and Virginia? How might this be related to
differences in land-use between the states?

**Answer 4:**

## Step 7 (Optional - Not graded): Analyzing multiple time periods

**Optional Question 5:**

Answer the following question by reusing and modifying the code above:
What was the probability of exceeding the EPA threshold for station
“05412500” in Iowa each year between April 1 and Sept 1 for years 2014
though 2018? (you will calculate the probability of exceeding separately
for each year). What do you need to modify to evaluate different years
rather than sites? Think about how the example above uses the `site`
vector and modify the code so that it loops through different time
periods.

Be sure your answer prints out to the document when you knit it.

**Optional Answer 5:**

``` r
#INSERT CODE

print(site_exceed)
```

**Optional Question 6:**

Is the probability of exceeding the EPA threshold changing over time?
Show with a plot.

**Optional Answer
6:**

``` r
ggplot(data = site_exceed_combined, mapping = aes(x = years, y = exceed)) +
  geom_line() +
  geom_point() +
  labs(x = "Year", y = "Proability of Exceeding EPA Threshold", 
       title = "Iowa: Proabability of nitrate exceeding EPA threshold")
```

## Reference

This module was initially developed by

Castendyk, D. and Gibson, C. 30 June 2015. Project EDDIE: Water Quality.
Project EDDIE Module 6, Version 1.
<http://cemast.illinoisstate.edu/data-for-students/modules/water-quality.shtml>.
Module development was supported by NSF DEB 1245707.
