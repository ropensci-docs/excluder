# Example text-based metadata from simulated Qualtrics study

A dataset containing the metadata from a standard Qualtrics survey with
browser metadata collected and exported with "Use choice text". These
data were randomly generated using
[iptools::ip_random()](https://hrbrmstr.github.io/iptools/reference/ip_random.html)
and
[rgeolocate::ip2location()](https://cran.r-project.org/src/contrib/Archive/rgeolocate/)
functions.

## Usage

``` r
qualtrics_text
```

## Format

A data frame with 100 rows and 16 variables:

- StartDate:

  date and time data collection started, in ISO 8601 format

- EndDate:

  date and time data collection ended, in ISO 8601 format

- Status:

  flag for preview (Survey Preview) vs. implemented survey (IP Address)
  entries

- IPAddress:

  participant IP address (truncated for anonymity)

- Progress:

  percentage of survey completed

- Duration (in seconds):

  duration of time required to complete survey, in seconds

- Finished:

  logical for whether survey was completed (TRUE) or progress was \< 100
  (FALSE)

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

- Browser:

  user web browser type

- Version:

  user web browser version

- Operating System:

  user operating system

- Resolution:

  user screen resolution

## See also

Other data:
[`qualtrics_fetch`](https://docs.ropensci.org/excluder/reference/qualtrics_fetch.md),
[`qualtrics_fetch2`](https://docs.ropensci.org/excluder/reference/qualtrics_fetch2.md),
[`qualtrics_numeric`](https://docs.ropensci.org/excluder/reference/qualtrics_numeric.md),
[`qualtrics_raw`](https://docs.ropensci.org/excluder/reference/qualtrics_raw.md)
