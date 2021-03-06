Working with strings and writing functions
================
FREC 3004: Environmental Informatics
3/20/2020

\#Working with strings

Strings are set of characters that are combined into a single element of
a vector or array. For example, the following vector has three strings

``` r
string_vector <- c("20190831", "20190901", "20190902")
string_vector
```

    ## [1] "20190831" "20190901" "20190902"

Often there is information within a string that you want to analyze or
use. For example, the object `string_vector` is a vector of dates in a
format that is not commonly used. We are interested in separating the
components of each string so that we can figure out which year, month,
and day the string is trying to represent.

Fortunately, there is a set of functions that we can use in R to extract
information or modify strings in the `stringr` package that is part of
the tidyverse. The `stringr` cheatsheet can be found here:

<https://github.com/rstudio/cheatsheets/blob/master/strings.pdf>

  - We we know that the first 4 numbers are the year. We can use the
    `str_sub()` to extract the first 4 characters of the string

<!-- end list -->

``` r
str_sub(string_vector, start = 1, end = 4)
```

    ## [1] "2019" "2019" "2019"

likewise we can extract the month using

``` r
str_sub(string_vector, start = 5, end = 6)
```

    ## [1] "08" "09" "09"

  - There are many other functions extract particular sets of
    characters. For example the `str_extract()` function pulls out a
    specified string if present.

<!-- end list -->

``` r
str_extract(string_vector, "09")
```

    ## [1] NA   "09" "09"

  - You can check whether a string has a particular string within it

<!-- end list -->

``` r
str_detect(string_vector, "09")
```

    ## [1] FALSE  TRUE  TRUE

  - If you have a vector of string that you want to separate into parts
    and you know that they are separated by specific character (like a
    space - " "), you can use the `separate` function to separate the
    string. The `sep` is the separating character and the `into` is the
    name of the columns that you want to save each part.

<!-- end list -->

``` r
species <- tibble(species = c("Picea abies",
                              "Schefflera actinophylla",
                              "Betula alleghaniensis Britton"))

split_species <- species %>% 
separate(species, sep = " ", into = c("GENUS", "SPECIES", "Other"))
```

    ## Warning: Expected 3 pieces. Missing pieces filled with `NA` in 2 rows [1, 2].

``` r
split_species
```

    ## # A tibble: 3 x 3
    ##   GENUS      SPECIES        Other  
    ##   <chr>      <chr>          <chr>  
    ## 1 Picea      abies          <NA>   
    ## 2 Schefflera actinophylla   <NA>   
    ## 3 Betula     alleghaniensis Britton

The `str_split_fixed()` can do the same thing but it does not work as
well with piping and does not name the columns

``` r
split_species <- str_split_fixed(species$species, pattern = " ", n = 3)
split_species
```

    ##      [,1]         [,2]             [,3]     
    ## [1,] "Picea"      "abies"          ""       
    ## [2,] "Schefflera" "actinophylla"   ""       
    ## [3,] "Betula"     "alleghaniensis" "Britton"

There are many other functions that allow you to work with and
manupulate strings in R. I recommend looking up the `stringr` package
and the cheatsheet.

## Creating functions

While R has many functions that others have created for you to use, you
will often have a particular calculation that you want to do repeatly
that does not currently exist in an R package. This requires you to
write your own function.

Function creation look like this

``` r
sum_two_numbers <- function(a, b){
  c <- a + b
  return(c)
}
```

where the function named `sum_two_numbers` and it requires to inputs
(`a` and `b`) and returns one output (`c`).

Running the chunk of code only loads the function in memory so that it
is aviaalble for you to use

You need to call the function to use it

``` r
summed_numbers <- sum_two_numbers(a = 3, b = 1)
summed_numbers
```

    ## [1] 4
