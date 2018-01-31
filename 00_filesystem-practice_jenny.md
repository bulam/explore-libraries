00\_filesystem-practice\_jenny.R
================
bill
Wed Jan 31 14:05:29 2018

``` r
library(stringr)
library(tidyverse)
```

    ## Loading tidyverse: ggplot2
    ## Loading tidyverse: tibble
    ## Loading tidyverse: tidyr
    ## Loading tidyverse: readr
    ## Loading tidyverse: purrr
    ## Loading tidyverse: dplyr

    ## Warning: package 'tidyr' was built under R version 3.4.1

    ## Warning: package 'purrr' was built under R version 3.4.1

    ## Warning: package 'dplyr' was built under R version 3.4.1

    ## Conflicts with tidy packages ----------------------------------------------

    ## filter(): dplyr, stats
    ## lag():    dplyr, stats

``` r
## First attempt: Just get it to work ----

list.files("~/Desktop/day1_s1_explore-libraries")
```

    ## [1] "01_explore-libraries_comfy.R"   "01_explore-libraries_jenny.R"  
    ## [3] "01_explore-libraries_spartan.R"

``` r
list.files("~/Desktop/day1_s1_explore-libraries", pattern = "\\.R$")
```

    ## [1] "01_explore-libraries_comfy.R"   "01_explore-libraries_jenny.R"  
    ## [3] "01_explore-libraries_spartan.R"

``` r
list.files(
  "~/Desktop/day1_s1_explore-libraries",
  pattern = "\\.R$",
  full.names = TRUE
)
```

    ## [1] "/Users/bill/Desktop/day1_s1_explore-libraries/01_explore-libraries_comfy.R"  
    ## [2] "/Users/bill/Desktop/day1_s1_explore-libraries/01_explore-libraries_jenny.R"  
    ## [3] "/Users/bill/Desktop/day1_s1_explore-libraries/01_explore-libraries_spartan.R"

``` r
from_files <- list.files(
  "~/Desktop/day1_s1_explore-libraries",
  pattern = "\\.R$",
  full.names = TRUE
)

(to_files <- basename(from_files))
```

    ## [1] "01_explore-libraries_comfy.R"   "01_explore-libraries_jenny.R"  
    ## [3] "01_explore-libraries_spartan.R"

``` r
file.copy(from_files, to_files)
```

    ## [1] TRUE TRUE TRUE

``` r
list.files()
```

    ## [1] "00_filesystem-practice_jenny.html"    
    ## [2] "00_filesystem-practice_jenny.R"       
    ## [3] "00_filesystem-practice_jenny.spin.R"  
    ## [4] "00_filesystem-practice_jenny.spin.Rmd"
    ## [5] "01_explore-libraries_comfy.R"         
    ## [6] "01_explore-libraries_jenny.R"         
    ## [7] "01_explore-libraries_spartan.R"       
    ## [8] "explore-libraries.Rproj"              
    ## [9] "README.md"

``` r
## Clean it out, so we can refine ----
file.remove(to_files)
```

    ## [1] TRUE TRUE TRUE

``` r
list.files()
```

    ## [1] "00_filesystem-practice_jenny.html"    
    ## [2] "00_filesystem-practice_jenny.R"       
    ## [3] "00_filesystem-practice_jenny.spin.R"  
    ## [4] "00_filesystem-practice_jenny.spin.Rmd"
    ## [5] "explore-libraries.Rproj"              
    ## [6] "README.md"

``` r
## Copy again, tighter code ----
from_dir <- file.path("~", "Desktop", "day1_s1_explore-libraries")
from_files <- list.files(from_dir, pattern = "\\.R$", full.names = TRUE)
to_files <- basename(from_files)
file.copy(from_files, to_files)
```

    ## [1] TRUE TRUE TRUE

``` r
list.files()
```

    ## [1] "00_filesystem-practice_jenny.html"    
    ## [2] "00_filesystem-practice_jenny.R"       
    ## [3] "00_filesystem-practice_jenny.spin.R"  
    ## [4] "00_filesystem-practice_jenny.spin.Rmd"
    ## [5] "01_explore-libraries_comfy.R"         
    ## [6] "01_explore-libraries_jenny.R"         
    ## [7] "01_explore-libraries_spartan.R"       
    ## [8] "explore-libraries.Rproj"              
    ## [9] "README.md"

``` r
## Clean it out, so we can use fs ----
file.remove(to_files)
```

    ## [1] TRUE TRUE TRUE

``` r
list.files()
```

    ## [1] "00_filesystem-practice_jenny.html"    
    ## [2] "00_filesystem-practice_jenny.R"       
    ## [3] "00_filesystem-practice_jenny.spin.R"  
    ## [4] "00_filesystem-practice_jenny.spin.Rmd"
    ## [5] "explore-libraries.Rproj"              
    ## [6] "README.md"

``` r
## Copy again, using fs ----
library(fs)
(from_dir <- path_home("Desktop", "day1_s1_explore-libraries"))
```

    ## /Users/bill/Desktop/day1_s1_explore-libraries

``` r
(from_files <- dir_ls(from_dir, glob = "*.R"))
```

    ## /Users/bill/Desktop/day1_s1_explore-libraries/01_explore-libraries_comfy.R
    ## /Users/bill/Desktop/day1_s1_explore-libraries/01_explore-libraries_jenny.R
    ## /Users/bill/Desktop/day1_s1_explore-libraries/01_explore-libraries_spartan.R

``` r
(to_files <- path_file(from_files))
```

    ## 01_explore-libraries_comfy.R   01_explore-libraries_jenny.R   
    ## 01_explore-libraries_spartan.R

``` r
(out <- file_copy(from_files, to_files))
```

    ## 01_explore-libraries_comfy.R   01_explore-libraries_jenny.R   
    ## 01_explore-libraries_spartan.R

``` r
dir_ls()
```

    ## 00_filesystem-practice_jenny.R
    ## 00_filesystem-practice_jenny.html
    ## 00_filesystem-practice_jenny.spin.R
    ## 00_filesystem-practice_jenny.spin.Rmd
    ## 01_explore-libraries_comfy.R
    ## 01_explore-libraries_jenny.R
    ## 01_explore-libraries_spartan.R
    ## README.md
    ## explore-libraries.Rproj

``` r
dir_info()
```

    ## Warning in as.POSIXlt.POSIXct(x, tz): unknown timezone 'default/America/
    ## Los_Angeles'

    ## # A tibble: 9 x 18
    ##   path                                  type         size permissions
    ##   <fs::path>                            <fct> <fs::bytes> <fs::perms>
    ## 1 00_filesystem-practice_jenny.R        file        1.75K rw-r--r--  
    ## 2 00_filesystem-practice_jenny.html     file       703.8K rw-r--r--  
    ## 3 00_filesystem-practice_jenny.spin.R   file        1.75K rw-r--r--  
    ## 4 00_filesystem-practice_jenny.spin.Rmd file        1.85K rw-r--r--  
    ## 5 01_explore-libraries_comfy.R          file        1.12K rw-r--r--  
    ## 6 01_explore-libraries_jenny.R          file        1.54K rw-r--r--  
    ## 7 01_explore-libraries_spartan.R        file        1.33K rw-r--r--  
    ## 8 README.md                             file          136 rw-r--r--  
    ## 9 explore-libraries.Rproj               file          205 rw-r--r--  
    ## # ... with 14 more variables: modification_time <dttm>, user <chr>, group
    ## #   <chr>, device_id <dbl>, hard_links <dbl>, special_device_id <dbl>,
    ## #   inode <dbl>, block_size <dbl>, blocks <dbl>, flags <int>, generation
    ## #   <dbl>, access_time <dttm>, change_time <dttm>, birth_time <dttm>

``` r
## Sidebar: Why does Jenny name files this way? ----
library(tidyverse)
ft <- tibble(files = dir_ls(glob = "*.R"))
ft
```

    ## # A tibble: 5 x 1
    ##   files                              
    ##   <fs::path>                         
    ## 1 00_filesystem-practice_jenny.R     
    ## 2 00_filesystem-practice_jenny.spin.R
    ## 3 01_explore-libraries_comfy.R       
    ## 4 01_explore-libraries_jenny.R       
    ## 5 01_explore-libraries_spartan.R

``` r
ft %>%
  filter(str_detect(files, "explore"))
```

    ## # A tibble: 3 x 1
    ##   files                         
    ##   <fs::path>                    
    ## 1 01_explore-libraries_comfy.R  
    ## 2 01_explore-libraries_jenny.R  
    ## 3 01_explore-libraries_spartan.R

``` r
ft %>%
  mutate(files = path_ext_remove(files)) %>%
  separate(files, into = c("i", "topic", "flavor"), sep = "_")
```

    ## # A tibble: 5 x 3
    ##   i     topic               flavor    
    ## * <chr> <chr>               <chr>     
    ## 1 00    filesystem-practice jenny     
    ## 2 00    filesystem-practice jenny.spin
    ## 3 01    explore-libraries   comfy     
    ## 4 01    explore-libraries   jenny     
    ## 5 01    explore-libraries   spartan

``` r
dirs <- dir_ls(path_home("Desktop"), type = "directory")
(dt <- tibble(dirs = path_file(dirs)))
```

    ## # A tibble: 4 x 1
    ##   dirs                     
    ##   <fs::path>               
    ## 1 Reference books          
    ## 2 day1_s1_explore-libraries
    ## 3 day1_s2_copy-files       
    ## 4 explore-libraries

``` r
dt %>%
  separate(dirs, into = c("day", "session", "topic"), sep = "_")
```

    ## Warning: Too few values at 2 locations: 1, 4

    ## # A tibble: 4 x 3
    ##   day               session topic            
    ## * <chr>             <chr>   <chr>            
    ## 1 Reference books   <NA>    <NA>             
    ## 2 day1              s1      explore-libraries
    ## 3 day1              s2      copy-files       
    ## 4 explore-libraries <NA>    <NA>

``` r
## Principled use of delimiters --> meta-data can be recovered from filename

## Clean it out, so we reset for workshop ----
file_delete(to_files)
dir_ls()
```

    ## 00_filesystem-practice_jenny.R
    ## 00_filesystem-practice_jenny.html
    ## 00_filesystem-practice_jenny.spin.R
    ## 00_filesystem-practice_jenny.spin.Rmd
    ## README.md
    ## explore-libraries.Rproj
