# Remove two initial rows created in Qualtrics data

The `remove_label_rows()` function filters out the initial label rows
from datasets downloaded from [Qualtrics](https://www.qualtrics.com/)
surveys.

## Usage

``` r
remove_label_rows(x, convert = TRUE, rename = FALSE)
```

## Arguments

- x:

  Data frame (downloaded from Qualtrics).

- convert:

  Logical indicating whether to convert/coerce date, logical and numeric
  columns from the metadata.

- rename:

  Logical indicating whether to rename columns based on first row of
  data.

## Value

An object of the same type as `x` that excludes Qualtrics label rows and
with date, logical, and numeric metadata columns converted to the
correct data class.

## Details

The function (1) checks if the data set uses Qualtrics column names, (2)
checks if label rows are already used as column names, (3) removes label
rows if present, and (4) converts date, logical, and numeric metadata
columns to proper data type. Datasets imported using
[qualtRics::fetch_survey()](https://docs.ropensci.org/qualtRics/reference/fetch_survey.html)
should not need this function.

The `convert` argument only converts the *StartDate*, *EndDate*,
*RecordedDate*, *Progress*, *Finished*, *Duration (in seconds)*,
*LocationLatitude*, and *LocationLongitude* columns. To convert other
data columns, see
[`dplyr::mutate()`](https://dplyr.tidyverse.org/reference/mutate.html).

## Examples

``` r
# Remove label rows
data(qualtrics_raw)
df <- remove_label_rows(qualtrics_raw)
```
