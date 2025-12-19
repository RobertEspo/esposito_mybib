

# mybib

**Last Updated**: 2025-12-19 00:00:18  
**License**: Public Domain (CC-0)

Version controlled .bib files for my scholarly work, as well as some
light analyses and a complete reference list.

This repo now includes two main bib files: **publications_html.bib** and
**publications_latex.bib**.

I call these files in to other projects (e.g., cv, personal website)
when I want to print a list of references.

Initial commit is a direct copy from [Joseph
Casillas](https://github.com/jvcasillas/mybib).

------------------------------------------------------------------------

Load bibs and generate some useful files and dataframes:

``` r
# Load bib
bib <- suppressWarnings(
  ReadBib(
    here("publications_html.bib"), 
    check = FALSE
  )
)

# Create csv of citekeys
cite_key_list <- bind_cols(
  bib$key |> unlist() |> tibble::enframe(name = NULL), 
  bib$bibtype |> unlist() |> tibble::enframe(name = NULL), 
  bib$year |> unlist() |> tibble::enframe(name = NULL)
  ) |> 
  rename(citekey = value...1, type = value...2, year = value...3) |> 
  write_csv(here("cite_key_list.csv"))

# Set bib options for printing
BibOptions(
  bib.style = "authoryear", 
  style = "text", 
  max.names = 10, 
  first.inits = TRUE, 
  check.entries = FALSE
)

# Convert to dataframe for analyses
dat <- bib |> 
  as_tibble() |> 
  map_df(.f = HTMLdecode) |> 
  mutate(year = as.numeric(year))
```
