# Rename columns to match standard Qualtrics names

The `rename_columns()` function renames the metadata columns to match
standard Qualtrics names.

## Usage

``` r
rename_columns(x, alert = TRUE)
```

## Arguments

- x:

  Data frame (preferably imported from Qualtrics using {qualtRics}).

- alert:

  Logical indicating whether to alert user to the fact that the columns
  do not match the secondary labels and therefore cannot be renamed.

## Value

An object of the same type as `x` that has column names that match
standard Qualtrics names.

## Details

When importing Qualtrics data using
[`qualtRics::fetch_survey()`](https://docs.ropensci.org/qualtRics/reference/fetch_survey.html).
labels entered in Qualtrics questions are saved as 'subtitles' for
column names. Using
[`sjlabelled::get_label()`](https://strengejacke.github.io/sjlabelled/reference/get_label.html)
can make these secondary labels be the primary column names. However,
this results in a different set of names for the metadata columns than
is used in all of the `mark_()`, `check_()`, and `exclude_()` functions.
This function renames these columns to match the standard Qualtrics
names.

## See also

Other column name functions:
[`use_labels()`](https://docs.ropensci.org/excluder/reference/use_labels.md)

## Examples

``` r
# Rename columns
data(qualtrics_fetch)
qualtrics_renamed <- qualtrics_fetch %>%
  rename_columns()
names(qualtrics_fetch)
#>  [1] "StartDate"             "EndDate"               "Status"               
#>  [4] "IPAddress"             "Progress"              "Duration (in seconds)"
#>  [7] "Finished"              "RecordedDate"          "ResponseId"           
#> [10] "LocationLatitude"      "LocationLongitude"     "UserLanguage"         
#> [13] "Q1_Browser"            "Q1_Version"            "Q1_Operating System"  
#> [16] "Q1_Resolution"         "Q2"                   
names(qualtrics_renamed)
#>  [1] "StartDate"             "EndDate"               "Status"               
#>  [4] "IPAddress"             "Progress"              "Duration (in seconds)"
#>  [7] "Finished"              "RecordedDate"          "ResponseId"           
#> [10] "LocationLatitude"      "LocationLongitude"     "UserLanguage"         
#> [13] "Browser"               "Version"               "Operating System"     
#> [16] "Resolution"            "Q2"                   

# Alerts when columns cannot be renamed
data(qualtrics_numeric)
rename_columns(qualtrics_numeric)
#> ! The columns are already named correctly.

# Turn off alert
rename_columns(qualtrics_numeric, alert = FALSE)
```
