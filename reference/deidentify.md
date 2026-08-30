# Remove columns that could include identifiable information

The `deidentify()` function selects out columns from
[Qualtrics](https://www.qualtrics.com/) surveys that may include
identifiable information such as IP address, location, or computer
characteristics.

## Usage

``` r
deidentify(x, strict = TRUE)
```

## Arguments

- x:

  Data frame (downloaded from Qualtrics).

- strict:

  Logical indicating whether to use strict or non-strict level of
  deidentification. Strict removes computer information columns in
  addition to IP address and location.

## Value

An object of the same type as `x` that excludes Qualtrics columns with
identifiable information.

## Details

The function offers two levels of deidentification. The default strict
level removes columns associated with IP address and location and
computer information (browser type and version, operating system, and
screen resolution). The non-strict level removes only columns associated
with IP address and location.

Typically, deidentification should be used at the end of a processing
pipeline so that these columns can be used to exclude rows.

## Examples

``` r
names(qualtrics_numeric)
#>  [1] "StartDate"             "EndDate"               "Status"               
#>  [4] "IPAddress"             "Progress"              "Duration (in seconds)"
#>  [7] "Finished"              "RecordedDate"          "ResponseId"           
#> [10] "LocationLatitude"      "LocationLongitude"     "UserLanguage"         
#> [13] "Browser"               "Version"               "Operating System"     
#> [16] "Resolution"           

# Remove IP address, location, and computer information columns
deid <- deidentify(qualtrics_numeric)
names(deid)
#> [1] "StartDate"             "EndDate"               "Status"               
#> [4] "Progress"              "Duration (in seconds)" "Finished"             
#> [7] "RecordedDate"          "ResponseId"           

# Remove only IP address and location columns
deid2 <- deidentify(qualtrics_numeric, strict = FALSE)
names(deid2)
#>  [1] "StartDate"             "EndDate"               "Status"               
#>  [4] "Progress"              "Duration (in seconds)" "Finished"             
#>  [7] "RecordedDate"          "ResponseId"            "Browser"              
#> [10] "Version"               "Operating System"      "Resolution"           
```
