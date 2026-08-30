# Example numeric metadata imported with `qualtRics::fetch_survey()` from simulated Qualtrics study

A dataset containing the metadata from a standard Qualtrics survey with
browser metadata collected and exported with "Use numeric values". The
data were imported using
[`qualtRics::fetch_survey()`](https://docs.ropensci.org/qualtRics/reference/fetch_survey.html).
These data were randomly generated using
[iptools::ip_random()](https://hrbrmstr.github.io/iptools/reference/ip_random.html)
and
[rgeolocate::ip2location()](https://cran.r-project.org/src/contrib/Archive/rgeolocate/)
functions.

## Usage

``` r
qualtrics_fetch
```

## Format

A data frame with 100 rows and 17 variables:

- StartDate:

  date and time data collection started, in ISO 8601 format

- EndDate:

  date and time data collection ended, in ISO 8601 format

- Status:

  numeric flag for preview (1) vs. implemented survey (0) entries

- IPAddress:

  participant IP address (truncated for anonymity)

- Progress:

  percentage of survey completed

- Duration (in seconds):

  duration of time required to complete survey, in seconds

- Finished:

  numeric flag for whether survey was completed (1) or progress was \<
  100 (0)

- RecordedDate:

  date and time survey was recorded, in ISO 8601 format

- ResponseId:

  random ID for participants

- LocationLatitude:

  latitude geolocated from IP address

- LocationLongitude:

  longitude geolocated from IP address

- UserLanguage:

  language set in Qualtrics

- Q1_Browser:

  user web browser type

- Q1_Version:

  user web browser version

- Q1_Operating System:

  user operating system

- Q1_Resolution:

  user screen resolution

- Q2:

  response to question about whether the user liked the survey (1 = Yes,
  0 = No)

## See also

Other data:
[`qualtrics_fetch2`](https://docs.ropensci.org/excluder/reference/qualtrics_fetch2.md),
[`qualtrics_numeric`](https://docs.ropensci.org/excluder/reference/qualtrics_numeric.md),
[`qualtrics_raw`](https://docs.ropensci.org/excluder/reference/qualtrics_raw.md),
[`qualtrics_text`](https://docs.ropensci.org/excluder/reference/qualtrics_text.md)
