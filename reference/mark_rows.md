# Return marked rows

Create new column marking rows that meet exclusion criteria. *This
function is not exported.*

## Usage

``` r
mark_rows(x, filtered_data, id_col, exclusion_type)
```

## Arguments

- x:

  Original data.

- filtered_data:

  Data to be excluded.

- id_col:

  Column name for unique row ID (e.g., participant).

- exclusion_type:

  Column name for exclusion column.
