# Check for locations outside of the US

The `check_location()` function subsets rows of data, retaining rows
that have locations outside of the US. The function is written to work
with data from [Qualtrics](https://www.qualtrics.com/) surveys.

## Usage

``` r
check_location(
  x,
  id_col = "ResponseId",
  location_col = c("LocationLatitude", "LocationLongitude"),
  rename = TRUE,
  include_na = FALSE,
  keep = FALSE,
  quiet = FALSE,
  print = TRUE
)
```

## Arguments

- x:

  Data frame (preferably imported from Qualtrics using {qualtRics}).

- id_col:

  Column name for unique row ID (e.g., participant).

- location_col:

  Two element vector specifying columns for latitude and longitude (in
  that order).

- rename:

  Logical indicating whether to rename columns (using
  [`rename_columns()`](https://docs.ropensci.org/excluder/reference/rename_columns.md))

- include_na:

  Logical indicating whether to include rows with NA in latitude and
  longitude columns in the output list of potentially excluded data.

- keep:

  Logical indicating whether to keep or remove exclusion column.

- quiet:

  Logical indicating whether to print message to console.

- print:

  Logical indicating whether to print returned tibble to console.

## Value

The output is a data frame of the rows that are located outside of the
US and (if `include_na == FALSE`) rows with no location information. For
a function that marks these rows, use
[`mark_location()`](https://docs.ropensci.org/excluder/reference/mark_location.md).
For a function that excludes these rows, use
[`exclude_location()`](https://docs.ropensci.org/excluder/reference/exclude_location.md).

## Details

To record this information in your Qualtrics survey, you must ensure
that [Anonymize responses is
disabled](https://www.qualtrics.com/support/survey-platform/survey-module/survey-options/survey-protection/#AnonymizingResponses).

Default column names are set based on output from the
[`qualtRics::fetch_survey()`](https://docs.ropensci.org/qualtRics/reference/fetch_survey.html).
The function only works for the United States. It uses the \#'
[`maps::map.where()`](https://rdrr.io/pkg/maps/man/map.where.html) to
determine if latitude and longitude are inside the US.

The function outputs to console a message about the number of rows with
locations outside of the US.

## See also

Other location functions:
[`exclude_location()`](https://docs.ropensci.org/excluder/reference/exclude_location.md),
[`mark_location()`](https://docs.ropensci.org/excluder/reference/mark_location.md)

Other check functions:
[`check_duplicates()`](https://docs.ropensci.org/excluder/reference/check_duplicates.md),
[`check_duration()`](https://docs.ropensci.org/excluder/reference/check_duration.md),
[`check_ip()`](https://docs.ropensci.org/excluder/reference/check_ip.md),
[`check_preview()`](https://docs.ropensci.org/excluder/reference/check_preview.md),
[`check_progress()`](https://docs.ropensci.org/excluder/reference/check_progress.md),
[`check_resolution()`](https://docs.ropensci.org/excluder/reference/check_resolution.md)

## Examples

``` r
# Check for locations outside of the US
data(qualtrics_text)
check_location(qualtrics_text)
#> ℹ 1 out of 100 rows had no information on location.
#> ℹ 5 out of 100 rows were located outside of the US.
#>             StartDate             EndDate     Status    IPAddress Progress
#> 1 2020-12-11 12:55:35 2020-12-11 12:59:24 IP Address 51.113.171.0      100
#> 2 2020-12-17 15:40:46 2020-12-17 15:45:20 IP Address  26.195.73.0      100
#> 3 2020-12-17 15:41:54 2020-12-17 15:47:24 IP Address  97.10.196.0      100
#> 4 2020-12-17 15:44:30 2020-12-17 15:47:57 IP Address   48.17.71.0      100
#> 5 2020-12-17 15:40:52 2020-12-17 15:49:08 IP Address   99.29.49.0      100
#> 6 2020-12-17 15:49:42 2020-12-17 15:51:50 IP Address 40.146.247.0        5
#>   Duration (in seconds) Finished        RecordedDate        ResponseId
#> 1                   296     TRUE 2020-12-11 12:59:24 R_Y27gVhACEH3aUUE
#> 2                   341     TRUE 2020-12-17 15:45:21 R_RCLqTKWdIen8A1Z
#> 3                   380     TRUE 2020-12-17 15:47:24 R_9bLdiaLyyfUlM7q
#> 4                   339     TRUE 2020-12-17 15:47:58 R_hEJfTQuUySzm9Ef
#> 5                   234     TRUE 2020-12-17 15:49:09 R_M8PpmDiJ7vXS9vO
#> 6                   307    FALSE 2020-12-17 15:51:50 R_HFKclPO5wWGNsFs
#>   LocationLatitude LocationLongitude UserLanguage       Browser       Version
#> 1         40.69746         -74.02277           EN        Chrome  87.0.4280.88
#> 2         42.25303         -71.00212           EN        Chrome  87.0.4280.88
#> 3               NA                NA           EN        Chrome  87.0.4280.88
#> 4         40.71187         -74.00837           EN Chrome iPhone  87.0.4280.77
#> 5         40.70961         -74.01063           EN        Chrome 87.0.4280.109
#> 6         29.29882         -81.04289           EN       Firefox          81.0
#>         Operating System Resolution
#> 1        Windows NT 10.0   1440x900
#> 2        Windows NT 10.0  1920x1080
#> 3        Windows NT 10.0   1536x864
#> 4                 iPhone    375x667
#> 5 CrOS x86_64 13505.73.0   1366x768
#> 6        Windows NT 10.0   1366x768

# Remove preview data first
qualtrics_text %>%
  exclude_preview() %>%
  check_location()
#> ℹ 2 out of 100 preview rows were excluded, leaving 98 rows.
#> ℹ 1 out of 98 rows had no information on location.
#> ℹ 5 out of 98 rows were located outside of the US.
#>             StartDate             EndDate     Status    IPAddress Progress
#> 1 2020-12-11 12:55:35 2020-12-11 12:59:24 IP Address 51.113.171.0      100
#> 2 2020-12-17 15:40:46 2020-12-17 15:45:20 IP Address  26.195.73.0      100
#> 3 2020-12-17 15:41:54 2020-12-17 15:47:24 IP Address  97.10.196.0      100
#> 4 2020-12-17 15:44:30 2020-12-17 15:47:57 IP Address   48.17.71.0      100
#> 5 2020-12-17 15:40:52 2020-12-17 15:49:08 IP Address   99.29.49.0      100
#> 6 2020-12-17 15:49:42 2020-12-17 15:51:50 IP Address 40.146.247.0        5
#>   Duration (in seconds) Finished        RecordedDate        ResponseId
#> 1                   296     TRUE 2020-12-11 12:59:24 R_Y27gVhACEH3aUUE
#> 2                   341     TRUE 2020-12-17 15:45:21 R_RCLqTKWdIen8A1Z
#> 3                   380     TRUE 2020-12-17 15:47:24 R_9bLdiaLyyfUlM7q
#> 4                   339     TRUE 2020-12-17 15:47:58 R_hEJfTQuUySzm9Ef
#> 5                   234     TRUE 2020-12-17 15:49:09 R_M8PpmDiJ7vXS9vO
#> 6                   307    FALSE 2020-12-17 15:51:50 R_HFKclPO5wWGNsFs
#>   LocationLatitude LocationLongitude UserLanguage       Browser       Version
#> 1         40.69746         -74.02277           EN        Chrome  87.0.4280.88
#> 2         42.25303         -71.00212           EN        Chrome  87.0.4280.88
#> 3               NA                NA           EN        Chrome  87.0.4280.88
#> 4         40.71187         -74.00837           EN Chrome iPhone  87.0.4280.77
#> 5         40.70961         -74.01063           EN        Chrome 87.0.4280.109
#> 6         29.29882         -81.04289           EN       Firefox          81.0
#>         Operating System Resolution
#> 1        Windows NT 10.0   1440x900
#> 2        Windows NT 10.0  1920x1080
#> 3        Windows NT 10.0   1536x864
#> 4                 iPhone    375x667
#> 5 CrOS x86_64 13505.73.0   1366x768
#> 6        Windows NT 10.0   1366x768

# Do not print rows to console
qualtrics_text %>%
  exclude_preview() %>%
  check_location(print = FALSE)
#> ℹ 2 out of 100 preview rows were excluded, leaving 98 rows.
#> ℹ 1 out of 98 rows had no information on location.
#> ℹ 5 out of 98 rows were located outside of the US.

# Do not print message to console
qualtrics_text %>%
  exclude_preview() %>%
  check_location(quiet = TRUE)
#> ℹ 2 out of 100 preview rows were excluded, leaving 98 rows.
#>             StartDate             EndDate     Status    IPAddress Progress
#> 1 2020-12-11 12:55:35 2020-12-11 12:59:24 IP Address 51.113.171.0      100
#> 2 2020-12-17 15:40:46 2020-12-17 15:45:20 IP Address  26.195.73.0      100
#> 3 2020-12-17 15:41:54 2020-12-17 15:47:24 IP Address  97.10.196.0      100
#> 4 2020-12-17 15:44:30 2020-12-17 15:47:57 IP Address   48.17.71.0      100
#> 5 2020-12-17 15:40:52 2020-12-17 15:49:08 IP Address   99.29.49.0      100
#> 6 2020-12-17 15:49:42 2020-12-17 15:51:50 IP Address 40.146.247.0        5
#>   Duration (in seconds) Finished        RecordedDate        ResponseId
#> 1                   296     TRUE 2020-12-11 12:59:24 R_Y27gVhACEH3aUUE
#> 2                   341     TRUE 2020-12-17 15:45:21 R_RCLqTKWdIen8A1Z
#> 3                   380     TRUE 2020-12-17 15:47:24 R_9bLdiaLyyfUlM7q
#> 4                   339     TRUE 2020-12-17 15:47:58 R_hEJfTQuUySzm9Ef
#> 5                   234     TRUE 2020-12-17 15:49:09 R_M8PpmDiJ7vXS9vO
#> 6                   307    FALSE 2020-12-17 15:51:50 R_HFKclPO5wWGNsFs
#>   LocationLatitude LocationLongitude UserLanguage       Browser       Version
#> 1         40.69746         -74.02277           EN        Chrome  87.0.4280.88
#> 2         42.25303         -71.00212           EN        Chrome  87.0.4280.88
#> 3               NA                NA           EN        Chrome  87.0.4280.88
#> 4         40.71187         -74.00837           EN Chrome iPhone  87.0.4280.77
#> 5         40.70961         -74.01063           EN        Chrome 87.0.4280.109
#> 6         29.29882         -81.04289           EN       Firefox          81.0
#>         Operating System Resolution
#> 1        Windows NT 10.0   1440x900
#> 2        Windows NT 10.0  1920x1080
#> 3        Windows NT 10.0   1536x864
#> 4                 iPhone    375x667
#> 5 CrOS x86_64 13505.73.0   1366x768
#> 6        Windows NT 10.0   1366x768
```
