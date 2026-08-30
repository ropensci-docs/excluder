# Use Qualtrics labels as column names

The `use_labels()` function renames the columns using the labels
generated in [Qualtrics](https://www.qualtrics.com/). Data must be
imported using
[`qualtRics::fetch_survey()`](https://docs.ropensci.org/qualtRics/reference/fetch_survey.html).

## Usage

``` r
use_labels(x)
```

## Arguments

- x:

  Data frame imported using `qualtRics::fetch_survey()`.

## Value

An object of the same type as `x` that has column names using the labels
generated in Qualtrics.

## See also

Other column name functions:
[`rename_columns()`](https://docs.ropensci.org/excluder/reference/rename_columns.md)

## Examples

``` r
# Rename columns
data(qualtrics_fetch)
qualtrics_renamed <- qualtrics_fetch %>%
  use_labels()
names(qualtrics_fetch)
#>  [1] "StartDate"             "EndDate"               "Status"               
#>  [4] "IPAddress"             "Progress"              "Duration (in seconds)"
#>  [7] "Finished"              "RecordedDate"          "ResponseId"           
#> [10] "LocationLatitude"      "LocationLongitude"     "UserLanguage"         
#> [13] "Q1_Browser"            "Q1_Version"            "Q1_Operating System"  
#> [16] "Q1_Resolution"         "Q2"                   
names(qualtrics_renamed)
#>  [1] "Start Date"            "End Date"              "Response Type"        
#>  [4] "IP Address"            "Progress"              "Duration (in seconds)"
#>  [7] "Finished"              "Recorded Date"         "Response ID"          
#> [10] "Location Latitude"     "Location Longitude"    "User Language"        
#> [13] "Browser"               "Version"               "Operating System"     
#> [16] "Resolution"            "like"                 
```
