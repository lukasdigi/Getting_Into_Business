README
================
Lukas DiGiovanni
2025-02-26

## Uploading the Dataset

``` r
# Install required packages if not already installed
if (!require(curl)) install.packages("curl", dependencies = TRUE)
```

    ## Loading required package: curl

    ## Using libcurl 8.11.1 with OpenSSL/3.3.2

``` r
if (!require(utils)) install.packages("utils", dependencies = TRUE)

# Load the packages
library(curl)
library(utils)

# Define the URL from Kaggle
dataset_url <- "https://www.kaggle.com/api/v1/datasets/download/fratzcan/usa-house-prices"

# Set the output file paths
zip_file <- "./usa-house-prices.zip"
extract_dir <- "./usa-house-prices"

# Download the ZIP file
curl_download(dataset_url, zip_file)

# Create extraction directory if it doesn't exist
if (!dir.exists(extract_dir)) dir.create(extract_dir)

# Unzip the file
unzip(zip_file, exdir = extract_dir)

# List extracted files
extracted_files <- list.files(extract_dir, full.names = TRUE)
message("Download and extraction complete! Files saved at: ", extract_dir)
```

    ## Download and extraction complete! Files saved at: ./usa-house-prices

``` r
message("Extracted files: ", paste(extracted_files, collapse = ", "))
```

    ## Extracted files: ./usa-house-prices/USA Housing Dataset.csv

``` r
setwd("/Users/lukasdigiovanni/Desktop/DS_201")

data <- read.csv("USA Housing Dataset.csv")
```

## Dataset Review

This code is used to find the date range of the dataset.

``` r
#Code to find the earliest and latest dates
data[["date"]] <- as.Date(data[["date"]])

# Find the earliest and latest dates
earliest_date <- min(data[["date"]], na.rm = TRUE)
latest_date <- max(data[["date"]], na.rm = TRUE)

# Print results
print(earliest_date)
```

    ## [1] "2014-05-02"

``` r
print(latest_date)
```

    ## [1] "2014-07-10"

**When was the data collected?** The data was collected in 2014. The
time range is from 5/2/2014 to 7/10/2014. The data was uploaded to
kaggle and last updated seven months ago.

**Where was the data acquired?** The data was acquired from kaggle.com.
The data found on that website was originally on zillow.com, the real
estate website.

**How was the data acquired?** Zillow’s Economic Research Team gathers,
refines, and publishes housing and economic data from both public and
proprietary sources. The core of Zillow’s data is derived from public
property records filed with local municipalities, including deeds,
property details, parcel information, and transaction histories. Many of
the statistics in these datasets are calculated from raw property data.
The methodology behind these calculations is further explained in the
next section, where the dataset attributes are described.

**What are the attributes of this dataset?** The dataset consists of 18
attributes that describe various characteristics of properties. The
Price variable represents the sale price of the property in USD and
serves as the target variable. Property features such as Bedrooms,
Bathrooms, Sqft Living, Sqft Lot, and Floors provide insight into the
size and layout of the home. The View and Condition variables are
ordinal indices that rate the property’s visual appeal and overall
state. The Waterfront attribute is a nominal, binary indicator of
whether the property has a waterfront view. Additionally, the dataset
includes Yr Built and Yr Renovated, which record the year the property
was originally constructed and last updated. Location-based attributes,
including Street, City, Statezip, and Country, offer contextual
information on where the property is situated.

**What type of data do these attributes contain?** The dataset contains
a mix of nominal, ordinal, interval, and ratio data types. Nominal
variables include categorical data without an inherent order, such as
Street, City, Statezip, Country, and Waterfront. Ordinal variables
represent ranked attributes with meaningful order but uneven intervals,
such as View, Condition, and Date. Interval data, such as Yr Built and
Yr Renovated, have meaningful differences between values but no true
zero point. Finally, Ratio data includes numerical attributes with a
true zero, such as Price, Bedrooms, Bathrooms, Sqft Living, Sqft Lot,
Floors, Sqft Above, and Sqft Basement, allowing for meaningful
comparisons and calculations.

## Data Dictionary

This code creates a table that shows the meaning of each variable
included in the dataset.

``` r
#import data dictionary
data_dictionary = read.csv("data_dictionary.csv")

library(knitr)
library(kableExtra)

# Create a nice table with kable and kableExtra
kable(data_dictionary, caption = "Variable Descriptions", col.names = c("Variable", "Description", "Type")) %>%
  kable_styling(bootstrap_options = c("striped", "hover", "condensed"), full_width = FALSE) %>%
  column_spec(1, bold = TRUE) %>%
  column_spec(2, italic = TRUE) %>%
  row_spec(0, background = "lightgray")  # Add a gray background to the header
```

<table class="table table-striped table-hover table-condensed" style="width: auto !important; margin-left: auto; margin-right: auto;">
<caption>
Variable Descriptions
</caption>
<thead>
<tr>
<th style="text-align:left;background-color: lightgray !important;">
Variable
</th>
<th style="text-align:left;background-color: lightgray !important;">
Description
</th>
<th style="text-align:left;background-color: lightgray !important;">
Type
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left;font-weight: bold;">
Date
</td>
<td style="text-align:left;font-style: italic;">
The date when the property was sold. This feature helps in understanding
the temporal trends in property prices.
</td>
<td style="text-align:left;">
Ordinal
</td>
</tr>
<tr>
<td style="text-align:left;font-weight: bold;">
Price
</td>
<td style="text-align:left;font-style: italic;">
The sale price of the property in USD. This is the target variable we
aim to predict.
</td>
<td style="text-align:left;">
Ratio
</td>
</tr>
<tr>
<td style="text-align:left;font-weight: bold;">
Bedrooms
</td>
<td style="text-align:left;font-style: italic;">
The number of bedrooms in the property. Generally, properties withmore
bedrooms tend to have higher prices.
</td>
<td style="text-align:left;">
Ratio
</td>
</tr>
<tr>
<td style="text-align:left;font-weight: bold;">
Bathrooms
</td>
<td style="text-align:left;font-style: italic;">
The number of bathrooms in the property. Similar to bedrooms, more
bathrooms can increase a property’s value.
</td>
<td style="text-align:left;">
Ratio
</td>
</tr>
<tr>
<td style="text-align:left;font-weight: bold;">
Sqft Living
</td>
<td style="text-align:left;font-style: italic;">
The size of the living area in square feet. Larger living areas are
typically associated with higher property values.
</td>
<td style="text-align:left;">
Ratio
</td>
</tr>
<tr>
<td style="text-align:left;font-weight: bold;">
Sqft Lot
</td>
<td style="text-align:left;font-style: italic;">
The size of the lot in square feet. Larger lots may increase a
property’s desirability and value.
</td>
<td style="text-align:left;">
Ratio
</td>
</tr>
<tr>
<td style="text-align:left;font-weight: bold;">
Floors
</td>
<td style="text-align:left;font-style: italic;">
The number of floors in the property. Properties with multiple floors
may offer more living space and appeal.
</td>
<td style="text-align:left;">
Ratio
</td>
</tr>
<tr>
<td style="text-align:left;font-weight: bold;">
Waterfront
</td>
<td style="text-align:left;font-style: italic;">
A binary indicator (1 if the property has a waterfront view, 0
other-wise). Properties with waterfront views are often valued higher.
</td>
<td style="text-align:left;">
Nominal
</td>
</tr>
<tr>
<td style="text-align:left;font-weight: bold;">
View
</td>
<td style="text-align:left;font-style: italic;">
An index from 0 to 4 indicating the quality of the property’s view.
Better views are likely to enhance a property’s value.
</td>
<td style="text-align:left;">
Ordinal
</td>
</tr>
<tr>
<td style="text-align:left;font-weight: bold;">
Condition
</td>
<td style="text-align:left;font-style: italic;">
An index from 1 to 5 rating the condition of the property. Properties in
better condition are typically worth more.
</td>
<td style="text-align:left;">
Ordinal
</td>
</tr>
<tr>
<td style="text-align:left;font-weight: bold;">
Sqft Above
</td>
<td style="text-align:left;font-style: italic;">
The square footage of the property above the basement. This can help
isolate the value contribution of above-ground space.
</td>
<td style="text-align:left;">
Ratio
</td>
</tr>
<tr>
<td style="text-align:left;font-weight: bold;">
Sqft Basement
</td>
<td style="text-align:left;font-style: italic;">
The square footage of the basement. Basements may add value depending on
their usability.
</td>
<td style="text-align:left;">
Ratio
</td>
</tr>
<tr>
<td style="text-align:left;font-weight: bold;">
Yr Built
</td>
<td style="text-align:left;font-style: italic;">
The year the property was built. Older properties may have historical
value, while newer ones may offer modern amenities.
</td>
<td style="text-align:left;">
Interval
</td>
</tr>
<tr>
<td style="text-align:left;font-weight: bold;">
Yr Renovated
</td>
<td style="text-align:left;font-style: italic;">
The year the property was last renovated. Recent renovations can
increase a property’s appeal and value.
</td>
<td style="text-align:left;">
Interval
</td>
</tr>
<tr>
<td style="text-align:left;font-weight: bold;">
Street
</td>
<td style="text-align:left;font-style: italic;">
The street address of the property. This feature can be used to analyze
location-specific price trends.
</td>
<td style="text-align:left;">
Nominal
</td>
</tr>
<tr>
<td style="text-align:left;font-weight: bold;">
City
</td>
<td style="text-align:left;font-style: italic;">
he city where the property is located. Different cities have distinct
market dynamics.
</td>
<td style="text-align:left;">
Nominal
</td>
</tr>
<tr>
<td style="text-align:left;font-weight: bold;">
Statezip
</td>
<td style="text-align:left;font-style: italic;">
The state and zip code of the property. This feature provides regional
context for the property.
</td>
<td style="text-align:left;">
Nominal
</td>
</tr>
<tr>
<td style="text-align:left;font-weight: bold;">
Country
</td>
<td style="text-align:left;font-style: italic;">
The country where the property is located. While this dataset focuses on
properties in Australia, this feature is included for completeness.
</td>
<td style="text-align:left;">
Nominal
</td>
</tr>
</tbody>
</table>

## Summary of Statistics

This table summarizes each variables data.

``` r
# Load necessary libraries
library(dplyr)
```

    ## 
    ## Attaching package: 'dplyr'

    ## The following object is masked from 'package:kableExtra':
    ## 
    ##     group_rows

    ## The following objects are masked from 'package:stats':
    ## 
    ##     filter, lag

    ## The following objects are masked from 'package:base':
    ## 
    ##     intersect, setdiff, setequal, union

``` r
library(readr)
```

    ## 
    ## Attaching package: 'readr'

    ## The following object is masked from 'package:curl':
    ## 
    ##     parse_date

``` r
library(kableExtra)

# Mode function
calculate_mode <- function(x) {
  unique_x <- na.omit(x)
  if (length(unique_x) == 0) return(NA)
  mode_value <- unique_x[which.max(tabulate(match(x, unique_x)))]
  return(mode_value)
}

# Use the existing 'data' dataframe loaded from "USA Housing Dataset.csv"
df <- data

# ----------------------------------------
# Remove specific variables from numeric summary
# ----------------------------------------
numeric_df <- df %>%
  select(where(is.numeric)) %>%
  select(-any_of(c("waterfront", "view", "condition")))

# Summary for numeric variables (excluding view, condition, waterfront)
numeric_summary <- numeric_df %>%
  reframe(
    Variable = names(.),
    Count = sapply(., function(x) sum(!is.na(x))),
    Missing = sapply(., function(x) sum(is.na(x))),
    Mean = sapply(., mean, na.rm = TRUE),
    SD = sapply(., sd, na.rm = TRUE),
    Min = sapply(., min, na.rm = TRUE),
    Q1 = sapply(., quantile, 0.25, na.rm = TRUE),
    Median = sapply(., median, na.rm = TRUE),
    Q3 = sapply(., quantile, 0.75, na.rm = TRUE),
    Max = sapply(., max, na.rm = TRUE)
  )

# ----------------------------------------
# Binary Summary for 'waterfront'
# ----------------------------------------
binary_summary <- df %>%
  select(waterfront) %>%
  reframe(
    Variable = "waterfront",
    Count = sum(!is.na(waterfront)),
    Missing = sum(is.na(waterfront)),
    Mode = calculate_mode(waterfront)
  )

# ----------------------------------------
# Interval-like Summary for 'condition' and 'view'
# (no mean/SD, just quantiles)
# ----------------------------------------
interval_df <- df %>% select(condition, view)

interval_summary <- interval_df %>%
  reframe(
    Variable = names(.),
    Count = sapply(., function(x) sum(!is.na(x))),
    Missing = sapply(., function(x) sum(is.na(x))),
    Min = sapply(., min, na.rm = TRUE),
    Q1 = sapply(., quantile, 0.25, na.rm = TRUE),
    Median = sapply(., median, na.rm = TRUE),
    Q3 = sapply(., quantile, 0.75, na.rm = TRUE),
    Max = sapply(., max, na.rm = TRUE)
  )

# ----------------------------------------
# Render all tables
# ----------------------------------------

# Numeric
numeric_summary %>%
  kable(format = "html", digits = 2, caption = "Summary Statistics - Numeric Variables") %>%
  kable_styling(full_width = FALSE, bootstrap_options = c("striped", "hover", "condensed", "responsive")) %>%
  column_spec(1, bold = TRUE)
```

<table class="table table-striped table-hover table-condensed table-responsive" style="width: auto !important; margin-left: auto; margin-right: auto;">
<caption>
Summary Statistics - Numeric Variables
</caption>
<thead>
<tr>
<th style="text-align:left;">
Variable
</th>
<th style="text-align:right;">
Count
</th>
<th style="text-align:right;">
Missing
</th>
<th style="text-align:right;">
Mean
</th>
<th style="text-align:right;">
SD
</th>
<th style="text-align:right;">
Min
</th>
<th style="text-align:right;">
Q1
</th>
<th style="text-align:right;">
Median
</th>
<th style="text-align:right;">
Q3
</th>
<th style="text-align:right;">
Max
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left;font-weight: bold;">
price
</td>
<td style="text-align:right;">
4140
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
553062.88
</td>
<td style="text-align:right;">
583686.45
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
320000.00
</td>
<td style="text-align:right;">
460000.00
</td>
<td style="text-align:right;">
659125.0
</td>
<td style="text-align:right;">
26590000.00
</td>
</tr>
<tr>
<td style="text-align:left;font-weight: bold;">
bedrooms
</td>
<td style="text-align:right;">
4140
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
3.40
</td>
<td style="text-align:right;">
0.90
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
3.00
</td>
<td style="text-align:right;">
3.00
</td>
<td style="text-align:right;">
4.0
</td>
<td style="text-align:right;">
8.00
</td>
</tr>
<tr>
<td style="text-align:left;font-weight: bold;">
bathrooms
</td>
<td style="text-align:right;">
4140
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
2.16
</td>
<td style="text-align:right;">
0.78
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
1.75
</td>
<td style="text-align:right;">
2.25
</td>
<td style="text-align:right;">
2.5
</td>
<td style="text-align:right;">
6.75
</td>
</tr>
<tr>
<td style="text-align:left;font-weight: bold;">
sqft_living
</td>
<td style="text-align:right;">
4140
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
2143.64
</td>
<td style="text-align:right;">
957.48
</td>
<td style="text-align:right;">
370
</td>
<td style="text-align:right;">
1470.00
</td>
<td style="text-align:right;">
1980.00
</td>
<td style="text-align:right;">
2620.0
</td>
<td style="text-align:right;">
10040.00
</td>
</tr>
<tr>
<td style="text-align:left;font-weight: bold;">
sqft_lot
</td>
<td style="text-align:right;">
4140
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
14697.64
</td>
<td style="text-align:right;">
35876.84
</td>
<td style="text-align:right;">
638
</td>
<td style="text-align:right;">
5000.00
</td>
<td style="text-align:right;">
7676.00
</td>
<td style="text-align:right;">
11000.0
</td>
<td style="text-align:right;">
1074218.00
</td>
</tr>
<tr>
<td style="text-align:left;font-weight: bold;">
floors
</td>
<td style="text-align:right;">
4140
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
1.51
</td>
<td style="text-align:right;">
0.53
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1.00
</td>
<td style="text-align:right;">
1.50
</td>
<td style="text-align:right;">
2.0
</td>
<td style="text-align:right;">
3.50
</td>
</tr>
<tr>
<td style="text-align:left;font-weight: bold;">
sqft_above
</td>
<td style="text-align:right;">
4140
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
1831.35
</td>
<td style="text-align:right;">
861.38
</td>
<td style="text-align:right;">
370
</td>
<td style="text-align:right;">
1190.00
</td>
<td style="text-align:right;">
1600.00
</td>
<td style="text-align:right;">
2310.0
</td>
<td style="text-align:right;">
8020.00
</td>
</tr>
<tr>
<td style="text-align:left;font-weight: bold;">
sqft_basement
</td>
<td style="text-align:right;">
4140
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
312.29
</td>
<td style="text-align:right;">
464.35
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
0.00
</td>
<td style="text-align:right;">
0.00
</td>
<td style="text-align:right;">
602.5
</td>
<td style="text-align:right;">
4820.00
</td>
</tr>
<tr>
<td style="text-align:left;font-weight: bold;">
yr_built
</td>
<td style="text-align:right;">
4140
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
1970.81
</td>
<td style="text-align:right;">
29.81
</td>
<td style="text-align:right;">
1900
</td>
<td style="text-align:right;">
1951.00
</td>
<td style="text-align:right;">
1976.00
</td>
<td style="text-align:right;">
1997.0
</td>
<td style="text-align:right;">
2014.00
</td>
</tr>
<tr>
<td style="text-align:left;font-weight: bold;">
yr_renovated
</td>
<td style="text-align:right;">
4140
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
808.37
</td>
<td style="text-align:right;">
979.38
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
0.00
</td>
<td style="text-align:right;">
0.00
</td>
<td style="text-align:right;">
1999.0
</td>
<td style="text-align:right;">
2014.00
</td>
</tr>
</tbody>
</table>

``` r
# Binary
binary_summary %>%
  kable(format = "html", caption = "Summary Statistics - Binary Variable: Waterfront") %>%
  kable_styling(full_width = FALSE, bootstrap_options = c("striped", "hover", "condensed", "responsive")) %>%
  column_spec(1, bold = TRUE)
```

<table class="table table-striped table-hover table-condensed table-responsive" style="width: auto !important; margin-left: auto; margin-right: auto;">
<caption>
Summary Statistics - Binary Variable: Waterfront
</caption>
<thead>
<tr>
<th style="text-align:left;">
Variable
</th>
<th style="text-align:right;">
Count
</th>
<th style="text-align:right;">
Missing
</th>
<th style="text-align:right;">
Mode
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left;font-weight: bold;">
waterfront
</td>
<td style="text-align:right;">
4140
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
0
</td>
</tr>
</tbody>
</table>

``` r
# Interval-like
interval_summary %>%
  kable(format = "html", digits = 2, caption = "Summary Statistics - Ordinal/Interval Variables: Condition and View") %>%
  kable_styling(full_width = FALSE, bootstrap_options = c("striped", "hover", "condensed", "responsive")) %>%
  column_spec(1, bold = TRUE)
```

<table class="table table-striped table-hover table-condensed table-responsive" style="width: auto !important; margin-left: auto; margin-right: auto;">
<caption>
Summary Statistics - Ordinal/Interval Variables: Condition and View
</caption>
<thead>
<tr>
<th style="text-align:left;">
Variable
</th>
<th style="text-align:right;">
Count
</th>
<th style="text-align:right;">
Missing
</th>
<th style="text-align:right;">
Min
</th>
<th style="text-align:right;">
Q1
</th>
<th style="text-align:right;">
Median
</th>
<th style="text-align:right;">
Q3
</th>
<th style="text-align:right;">
Max
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left;font-weight: bold;">
condition
</td>
<td style="text-align:right;">
4140
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
4
</td>
<td style="text-align:right;">
5
</td>
</tr>
<tr>
<td style="text-align:left;font-weight: bold;">
view
</td>
<td style="text-align:right;">
4140
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
4
</td>
</tr>
</tbody>
</table>

## Boxplot Comparing Waterfront & Non-Waterfront

This code creates a boxplot that will show the prices of houses that are
on the water versus not on the water.

``` r
library(ggplot2)

# Create the boxplot with outliers
ggplot(data, aes(x = as.factor(waterfront), y = price)) +
  geom_boxplot(fill = c("skyblue", "orange"), alpha = 0.7, outlier.color = "red", outlier.shape = 16) +
  labs(
    x = "Waterfront (0 = No, 1 = Yes)",
    y = "House Price",
    title = "House Prices: Waterfront vs. Non-Waterfront (With Outliers)"
  ) +
  theme_minimal()
```

![](README_files/figure-gfm/unnamed-chunk-6-1.png)<!-- -->

**Analysis** This visual is difficult to analyze because of the scale of
the data. There are clearly some outliers included that, when removed,
may make it easier to look for insight in the data.

## Outlier Analysis

This code is used to calculate outliers and average them for waterfron
and non-waterfront houses.

``` r
library(dplyr)

# Calculate IQR for non-waterfront houses (waterfront = 0)
non_waterfront_stats <- data %>%
  filter(waterfront == 0) %>%
  summarise(
    Q1 = quantile(price, 0.25, na.rm = TRUE),
    Q3 = quantile(price, 0.75, na.rm = TRUE),
    IQR = Q3 - Q1,
    Lower_Bound = Q1 - 1.5 * IQR,
    Upper_Bound = Q3 + 1.5 * IQR
  )

# Identify outliers and non-outliers
non_waterfront_outliers <- data %>%
  filter(waterfront == 0 & (price < non_waterfront_stats$Lower_Bound | price > non_waterfront_stats$Upper_Bound))

non_waterfront_non_outliers <- data %>%
  filter(waterfront == 0 & (price >= non_waterfront_stats$Lower_Bound & price <= non_waterfront_stats$Upper_Bound))

# Calculate the average sqft_lot
avg_sqft_lot_outliers <- mean(non_waterfront_outliers$sqft_lot, na.rm = TRUE)
avg_sqft_lot_non_outliers <- mean(non_waterfront_non_outliers$sqft_lot, na.rm = TRUE)

# Print results
cat("Average sqft_lot of non-waterfront outliers:", avg_sqft_lot_outliers, "\n")
```

    ## Average sqft_lot of non-waterfront outliers: 18708.4

``` r
cat("Average sqft_lot of non-waterfront non-outliers:", avg_sqft_lot_non_outliers, "\n")
```

    ## Average sqft_lot of non-waterfront non-outliers: 14425.07

This shows the massive difference between non-waterfront house outliers
and non-outliers. The outliers have much more square footage on average
than the non-waterfront houses.

## Updated Box Plot

This plot now showcases the data without the outliers

``` r
library(ggplot2)
library(dplyr)

# Calculate IQR and filter out outliers
filtered_data <- data %>% 
  group_by(waterfront) %>% 
  mutate(
    Q1 = quantile(price, 0.25, na.rm = TRUE),
    Q3 = quantile(price, 0.75, na.rm = TRUE),
    IQR = Q3 - Q1,
    Lower_Bound = Q1 - 1.5 * IQR,
    Upper_Bound = Q3 + 1.5 * IQR
  ) %>%
  filter(price >= Lower_Bound & price <= Upper_Bound) %>%
  ungroup()

# Create the boxplot without outliers
ggplot(filtered_data, aes(x = as.factor(waterfront), y = price)) +
  geom_boxplot(fill = c("skyblue", "orange"), alpha = 0.7) +
  labs(
    x = "Waterfront (0 = No, 1 = Yes)",
    y = "House Price",
    title = "House Prices: Waterfront vs. Non-Waterfront (Without Outliers)"
  ) +
  theme_minimal()
```

![](README_files/figure-gfm/unnamed-chunk-8-1.png)<!-- -->

### Boxplot Interpretation

The boxplot compares house prices between waterfront and non-waterfront
properties:

1.  **Price Distribution**:
    - Waterfront properties (1) have a significantly higher median house
      price compared to non-waterfront properties (0).
    - The interquartile range (IQR) for waterfront properties is larger,
      indicating more variation in prices.
2.  **Outliers**:
    - Non-waterfront properties show some high-price outliers, but the
      overall price range is much lower.
    - Waterfront properties have a higher maximum price, even when
      outliers are removed.
3.  **Conclusion**:
    - The data suggests that being located on the waterfront is a strong
      predictor of higher house prices.
    - The price disparity indicates that buyers may be willing to pay a
      premium for waterfront locations.

# Additional Data Source for Investment Strategy

## Real Estate Market Trends Dataset

One useful additional dataset for informing an investment strategy would
be a real estate market trends dataset, such as **Zillow’s Housing
Data** or the **Federal Housing Finance Agency (FHFA) House Price
Index**.

### Why would this dataset be useful?

This dataset would provide insights into housing price trends over time,
mortgage rates, and regional market fluctuations. Understanding
historical trends can help predict future property values and assess
whether a given area is appreciating or depreciating in value.

### How could it complement the data you are currently analyzing?

The current dataset focuses on specific property features (e.g.,
waterfront status) and their impact on prices. However, macroeconomic
factors, such as interest rates, inflation, and supply-demand dynamics,
also influence real estate prices. A market trends dataset would provide
a broader economic context to better assess investment potential.

### Additional Dataset Link:

- [Zillow Research Data](https://www.zillow.com/research/data/)
