# Example numeric metadata imported with `qualtRics::fetch_survey()` from simulated Qualtrics study but with labels included as column names

A dataset containing the metadata from a standard Qualtrics survey with
browser metadata collected and exported with "Use numeric values". The
data were imported using
[`qualtRics::fetch_survey()`](https://docs.ropensci.org/qualtRics/reference/fetch_survey.html).
and then the secondary labels were assigned as column names with
[`sjlabelled::get_label()`](https://strengejacke.github.io/sjlabelled/reference/get_label.html).
These data were randomly generated using
[iptools::ip_random()](https://hrbrmstr.github.io/iptools/reference/ip_random.html)
and
[rgeolocate::ip2location()](https://cran.r-project.org/src/contrib/Archive/rgeolocate/)
functions.

## Usage

``` r
qualtrics_fetch2
```

## Format

A data frame with 100 rows and 17 variables:

- Start Date:

  date and time data collection started, in ISO 8601 format

- End Date:

  date and time data collection ended, in ISO 8601 format

- Response Type:

  numeric flag for preview (1) vs. implemented survey (0) entries

- IP Address:

  participant IP address (truncated for anonymity)

- Progress:

  percentage of survey completed

- Duration (in seconds):

  duration of time required to complete survey, in seconds

- Finished:

  numeric flag for whether survey was completed (1) or progress was \<
  100 (0)

- Recorded Date:

  date and time survey was recorded, in ISO 8601 format

- Response ID:

  random ID for participants

- Location Latitude:

  latitude geolocated from IP address

- Location Longitude:

  longitude geolocated from IP address

- User Language:

  language set in Qualtrics

- Click to write the question text - Browser:

  user web browser type

- Click to write the question text - Version:

  user web browser version

- Click to write the question text - Operating System:

  user operating system

- Click to write the question text - Resolution:

  user screen resolution

- like:

  response to question about whether the user liked the survey (1 = Yes,
  0 = No)

## See also

Other data:
[`qualtrics_fetch`](https://docs.ropensci.org/excluder/reference/qualtrics_fetch.md),
[`qualtrics_numeric`](https://docs.ropensci.org/excluder/reference/qualtrics_numeric.md),
[`qualtrics_raw`](https://docs.ropensci.org/excluder/reference/qualtrics_raw.md),
[`qualtrics_text`](https://docs.ropensci.org/excluder/reference/qualtrics_text.md)
