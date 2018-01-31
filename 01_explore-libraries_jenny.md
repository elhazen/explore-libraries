01\_explore-libraries\_jenny.R
================
elliotthazen
Wed Jan 31 14:41:40 2018

``` r
library(fs)
library(tidyverse)
```

    ## ── Attaching packages ──────────────────────────────────────────────────────────────────────────────────────────────────── tidyverse 1.2.1 ──

    ## ✔ ggplot2 2.2.1.9000     ✔ purrr   0.2.4     
    ## ✔ tibble  1.4.2          ✔ dplyr   0.7.4     
    ## ✔ tidyr   0.7.2          ✔ stringr 1.2.0     
    ## ✔ readr   1.1.1          ✔ forcats 0.2.0

    ## ── Conflicts ─────────────────────────────────────────────────────────────────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()

``` r
library(stringr)
library(igraph)
```

    ## 
    ## Attaching package: 'igraph'

    ## The following objects are masked from 'package:dplyr':
    ## 
    ##     as_data_frame, groups, union

    ## The following objects are masked from 'package:purrr':
    ## 
    ##     compose, simplify

    ## The following object is masked from 'package:tidyr':
    ## 
    ##     crossing

    ## The following object is masked from 'package:tibble':
    ## 
    ##     as_data_frame

    ## The following object is masked from 'package:fs':
    ## 
    ##     path

    ## The following objects are masked from 'package:stats':
    ## 
    ##     decompose, spectrum

    ## The following object is masked from 'package:base':
    ## 
    ##     union

``` r
library(ggraph)
library(here)
```

    ## here() starts at /Users/elliotthazen/Documents/R/github/explore-libraries

``` r
## how jenny might do this in a first exploration
## purposely leaving a few things to change later!
```

Which libraries does R search for packages?

``` r
.libPaths()
```

    ## [1] "/Library/Frameworks/R.framework/Versions/3.3/Resources/library"

``` r
## let's confirm the second element is, in fact, the default library
.Library
```

    ## [1] "/Library/Frameworks/R.framework/Resources/library"

``` r
path_real(.Library)
```

    ## /Library/Frameworks/R.framework/Versions/3.3/Resources/library

Installed packages

``` r
ipt <- installed.packages() %>%
  as_tibble()

## how many packages?
nrow(ipt)
```

    ## [1] 272

Exploring the packages

``` r
## count some things! inspiration
##   * tabulate by LibPath, Priority, or both
ipt %>%
  count(LibPath, Priority)
```

    ## # A tibble: 3 x 3
    ##   LibPath                                                 Priority       n
    ##   <chr>                                                   <chr>      <int>
    ## 1 /Library/Frameworks/R.framework/Versions/3.3/Resources… base          14
    ## 2 /Library/Frameworks/R.framework/Versions/3.3/Resources… recommend…    15
    ## 3 /Library/Frameworks/R.framework/Versions/3.3/Resources… <NA>         243

``` r
##   * what proportion need compilation?
ipt %>%
  count(NeedsCompilation) %>%
  mutate(prop = n / sum(n))
```

    ## # A tibble: 3 x 3
    ##   NeedsCompilation     n   prop
    ##   <chr>            <int>  <dbl>
    ## 1 no                 106 0.390 
    ## 2 yes                142 0.522 
    ## 3 <NA>                24 0.0882

``` r
##   * how break down re: version of R they were built on
ipt %>%
  count(Built) %>%
  mutate(prop = n / sum(n)) 
```

    ## # A tibble: 2 x 3
    ##   Built     n  prop
    ##   <chr> <int> <dbl>
    ## 1 3.3.0    63 0.232
    ## 2 3.3.2   209 0.768

Reflections

``` r
## reflect on ^^ and make a few notes to yourself; inspiration
##   * does the number of base + recommended packages make sense to you?
##   * how does the result of .libPaths() relate to the result of .Library?
```

Going further

``` r
## if you have time to do more ...

## is every package in .Library either base or recommended?
all_default_pkgs <- list.files(.Library)
all_br_pkgs <- ipt %>%
  filter(Priority %in% c("base", "recommended")) %>%
  pull(Package)
setdiff(all_default_pkgs, all_br_pkgs)
```

    ##   [1] "acepack"           "ade4"              "adehabitat"       
    ##   [4] "adehabitatHR"      "adehabitatHS"      "adehabitatLT"     
    ##   [7] "adehabitatMA"      "akima"             "animation"        
    ##  [10] "assertthat"        "backports"         "base64enc"        
    ##  [13] "BH"                "bindr"             "bindrcpp"         
    ##  [16] "bitops"            "broom"             "callr"            
    ##  [19] "caTools"           "CCA"               "cellranger"       
    ##  [22] "checkmate"         "CircStats"         "classInt"         
    ##  [25] "cli"               "clipr"             "clisymbols"       
    ##  [28] "coda"              "colorRamps"        "colorspace"       
    ##  [31] "corpcor"           "covr"              "crayon"           
    ##  [34] "crosstalk"         "curl"              "data.table"       
    ##  [37] "DBI"               "dbplyr"            "deldir"           
    ##  [40] "Deriv"             "desc"              "devtools"         
    ##  [43] "dichromat"         "digest"            "doParallel"       
    ##  [46] "dotCall64"         "dplyr"             "e1071"            
    ##  [49] "enc"               "evaluate"          "fda"              
    ##  [52] "FGN"               "fields"            "filehash"         
    ##  [55] "FNN"               "forcats"           "foreach"          
    ##  [58] "Formula"           "fs"                "gamm4"            
    ##  [61] "gbm"               "gdtools"           "geosphere"        
    ##  [64] "GGally"            "gganimate"         "ggforce"          
    ##  [67] "ggmap"             "ggplot2"           "ggplot2movies"    
    ##  [70] "ggraph"            "ggrepel"           "ggsn"             
    ##  [73] "gh"                "GISTools"          "git2r"            
    ##  [76] "glue"              "gmt"               "gridBase"         
    ##  [79] "gridExtra"         "gstat"             "gtable"           
    ##  [82] "gtools"            "haven"             "here"             
    ##  [85] "hexbin"            "highr"             "HKprocess"        
    ##  [88] "Hmisc"             "hms"               "hoardr"           
    ##  [91] "htmlTable"         "htmltools"         "htmlwidgets"      
    ##  [94] "httpuv"            "httr"              "igraph"           
    ##  [97] "ini"               "INLA"              "intervals"        
    ## [100] "irlba"             "iterators"         "jpeg"             
    ## [103] "jsonlite"          "knitr"             "labeling"         
    ## [106] "latticeExtra"      "lazyeval"          "leaflet"          
    ## [109] "lme4"              "ltsa"              "lubridate"        
    ## [112] "lunar"             "magrittr"          "mapdata"          
    ## [115] "mapproj"           "maps"              "maptools"         
    ## [118] "markdown"          "MatrixModels"      "mcmc"             
    ## [121] "MCMCpack"          "memoise"           "mime"             
    ## [124] "minqa"             "misc3d"            "mixtools"         
    ## [127] "mnormt"            "modelr"            "multcomp"         
    ## [130] "munsell"           "mvtnorm"           "ncdf4"            
    ## [133] "nloptr"            "NMF"               "numDeriv"         
    ## [136] "openssl"           "orthopolynom"      "PBSmapping"       
    ## [139] "pillar"            "pixmap"            "pkgconfig"        
    ## [142] "pkgmaker"          "plogr"             "plot3D"           
    ## [145] "plotdap"           "plyr"              "png"              
    ## [148] "polynom"           "praise"            "prettymapr"       
    ## [151] "prettyunits"       "progress"          "proj4"            
    ## [154] "proto"             "psych"             "purrr"            
    ## [157] "quantreg"          "R.methodsS3"       "R.oo"             
    ## [160] "R.utils"           "R6"                "RandomFields"     
    ## [163] "RandomFieldsUtils" "RANN"              "rappdirs"         
    ## [166] "raster"            "rasterVis"         "RColorBrewer"     
    ## [169] "Rcpp"              "RcppEigen"         "RCurl"            
    ## [172] "readr"             "readxl"            "registry"         
    ## [175] "rematch"           "rematch2"          "reprex"           
    ## [178] "rerddap"           "reshape"           "reshape2"         
    ## [181] "rex"               "rgdal"             "rgeos"            
    ## [184] "rgl"               "RgoogleMaps"       "rjson"            
    ## [187] "rlang"             "rmarkdown"         "rngtools"         
    ## [190] "rprojroot"         "rstudioapi"        "RUnit"            
    ## [193] "rvest"             "sandwich"          "scales"           
    ## [196] "SDMTools"          "segmented"         "selectr"          
    ## [199] "sf"                "shape"             "shapefiles"       
    ## [202] "shiny"             "sourcetools"       "sp"               
    ## [205] "spacetime"         "spam"              "SparseM"          
    ## [208] "SpatialDeltaGLMM"  "SpatialDFA"        "splancs"          
    ## [211] "stabledist"        "statmod"           "stringi"          
    ## [214] "stringr"           "styler"            "svglite"          
    ## [217] "testthat"          "TH.data"           "ThorsonUtilities" 
    ## [220] "tibble"            "tidyr"             "tidyselect"       
    ## [223] "tidyverse"         "tkrplot"           "TMB"              
    ## [226] "TMBhelper"         "translations"      "tweedie"          
    ## [229] "tweenr"            "udunits2"          "units"            
    ## [232] "usethis"           "utf8"              "VAST"             
    ## [235] "viridis"           "viridisLite"       "whisker"          
    ## [238] "withr"             "XML"               "xml2"             
    ## [241] "xtable"            "xts"               "yaml"             
    ## [244] "zoo"

``` r
## study package naming style (all lower case, contains '.', etc

## use `fields` argument to installed.packages() to get more info and use it!
ipt2 <- installed.packages(fields = "URL") %>%
  as_tibble()
ipt2 %>%
  mutate(github = grepl("github", URL)) %>%
  count(github) %>%
  mutate(prop = n / sum(n))
```

    ## # A tibble: 2 x 3
    ##   github     n  prop
    ##   <lgl>  <int> <dbl>
    ## 1 F        157 0.577
    ## 2 T        115 0.423

Try out some plotting created here: <https://github.com/aedobbyn/what-they-forgot/blob/5aced5a4061848bfb58ece7e3d8a742550145ab3/day1_s1_explore-libraries/package_links.R>

``` r
# Tibble of installed packages
inst_packages <- installed.packages() %>% as_tibble()

# Take a look at what we've got in LinkingTo; seems like a comma separated string
inst_packages$LinkingTo[1:50]
```

    ##  [1] NA            NA            NA            NA            NA           
    ##  [6] NA            NA            NA            NA            NA           
    ## [11] NA            NA            NA            NA            NA           
    ## [16] "Rcpp, plogr" NA            NA            NA            NA           
    ## [21] NA            NA            NA            NA            NA           
    ## [26] NA            NA            NA            NA            NA           
    ## [31] NA            NA            NA            NA            NA           
    ## [36] NA            NA            NA            NA            NA           
    ## [41] NA            NA            NA            NA            NA           
    ## [46] NA            NA            NA            NA            NA

``` r
# For now, take just the first link and remove trailing commas
inst_packages <- inst_packages %>%
  mutate(
    linking_to = str_split(LinkingTo, " ") %>% map_chr(first) %>% gsub(",", "", .)
  )

# Create the links between packages and their first LinkingTo package
package_links <- inst_packages %>%
  drop_na(linking_to) %>%
  select(Package, linking_to) %>%
  as_tibble() %>%
  igraph::graph_from_data_frame()

# Make the graph!
link_graph <- ggraph::ggraph(package_links, layout = "fr") +
  geom_edge_link(alpha = 0.5) +
  geom_node_point(color = "blue", size = 5, alpha = 0.5) +
  geom_node_text(aes(label = name), repel = TRUE) +
  theme_void() +
  ggtitle("Packages LinkingTo other packages")

link_graph
```

![](01_explore-libraries_jenny_files/figure-markdown_github/unnamed-chunk-7-1.png)
