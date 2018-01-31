01\_explore-libraries\_jenny.R
================
elliotthazen
Wed Jan 31 14:05:24 2018

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
library(fs)
path_real(.Library)
```

    ## /Library/Frameworks/R.framework/Versions/3.3/Resources/library

Installed packages

``` r
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
ipt <- installed.packages() %>%
  as_tibble()

## how many packages?
nrow(ipt)
```

    ## [1] 268

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
    ## 3 /Library/Frameworks/R.framework/Versions/3.3/Resources… <NA>         239

``` r
##   * what proportion need compilation?
ipt %>%
  count(NeedsCompilation) %>%
  mutate(prop = n / sum(n))
```

    ## # A tibble: 3 x 3
    ##   NeedsCompilation     n   prop
    ##   <chr>            <int>  <dbl>
    ## 1 no                 106 0.396 
    ## 2 yes                138 0.515 
    ## 3 <NA>                24 0.0896

``` r
##   * how break down re: version of R they were built on
ipt %>%
  count(Built) %>%
  mutate(prop = n / sum(n))
```

    ## # A tibble: 2 x 3
    ##   Built     n  prop
    ##   <chr> <int> <dbl>
    ## 1 3.3.0    62 0.231
    ## 2 3.3.2   206 0.769

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
    ##  [64] "GGally"            "gganimate"         "ggmap"            
    ##  [67] "ggplot2"           "ggplot2movies"     "ggsn"             
    ##  [70] "gh"                "GISTools"          "git2r"            
    ##  [73] "glue"              "gmt"               "gridBase"         
    ##  [76] "gridExtra"         "gstat"             "gtable"           
    ##  [79] "gtools"            "haven"             "here"             
    ##  [82] "hexbin"            "highr"             "HKprocess"        
    ##  [85] "Hmisc"             "hms"               "hoardr"           
    ##  [88] "htmlTable"         "htmltools"         "htmlwidgets"      
    ##  [91] "httpuv"            "httr"              "igraph"           
    ##  [94] "ini"               "INLA"              "intervals"        
    ##  [97] "irlba"             "iterators"         "jpeg"             
    ## [100] "jsonlite"          "knitr"             "labeling"         
    ## [103] "latticeExtra"      "lazyeval"          "leaflet"          
    ## [106] "lme4"              "ltsa"              "lubridate"        
    ## [109] "lunar"             "magrittr"          "mapdata"          
    ## [112] "mapproj"           "maps"              "maptools"         
    ## [115] "markdown"          "MatrixModels"      "mcmc"             
    ## [118] "MCMCpack"          "memoise"           "mime"             
    ## [121] "minqa"             "misc3d"            "mixtools"         
    ## [124] "mnormt"            "modelr"            "multcomp"         
    ## [127] "munsell"           "mvtnorm"           "ncdf4"            
    ## [130] "nloptr"            "NMF"               "numDeriv"         
    ## [133] "openssl"           "orthopolynom"      "PBSmapping"       
    ## [136] "pillar"            "pixmap"            "pkgconfig"        
    ## [139] "pkgmaker"          "plogr"             "plot3D"           
    ## [142] "plotdap"           "plyr"              "png"              
    ## [145] "polynom"           "praise"            "prettymapr"       
    ## [148] "prettyunits"       "progress"          "proj4"            
    ## [151] "proto"             "psych"             "purrr"            
    ## [154] "quantreg"          "R.methodsS3"       "R.oo"             
    ## [157] "R.utils"           "R6"                "RandomFields"     
    ## [160] "RandomFieldsUtils" "RANN"              "rappdirs"         
    ## [163] "raster"            "rasterVis"         "RColorBrewer"     
    ## [166] "Rcpp"              "RcppEigen"         "RCurl"            
    ## [169] "readr"             "readxl"            "registry"         
    ## [172] "rematch"           "rematch2"          "reprex"           
    ## [175] "rerddap"           "reshape"           "reshape2"         
    ## [178] "rex"               "rgdal"             "rgeos"            
    ## [181] "rgl"               "RgoogleMaps"       "rjson"            
    ## [184] "rlang"             "rmarkdown"         "rngtools"         
    ## [187] "rprojroot"         "rstudioapi"        "RUnit"            
    ## [190] "rvest"             "sandwich"          "scales"           
    ## [193] "SDMTools"          "segmented"         "selectr"          
    ## [196] "sf"                "shape"             "shapefiles"       
    ## [199] "shiny"             "sourcetools"       "sp"               
    ## [202] "spacetime"         "spam"              "SparseM"          
    ## [205] "SpatialDeltaGLMM"  "SpatialDFA"        "splancs"          
    ## [208] "stabledist"        "statmod"           "stringi"          
    ## [211] "stringr"           "styler"            "svglite"          
    ## [214] "testthat"          "TH.data"           "ThorsonUtilities" 
    ## [217] "tibble"            "tidyr"             "tidyselect"       
    ## [220] "tidyverse"         "tkrplot"           "TMB"              
    ## [223] "TMBhelper"         "translations"      "tweedie"          
    ## [226] "udunits2"          "units"             "usethis"          
    ## [229] "utf8"              "VAST"              "viridis"          
    ## [232] "viridisLite"       "whisker"           "withr"            
    ## [235] "XML"               "xml2"              "xtable"           
    ## [238] "xts"               "yaml"              "zoo"

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
    ## 1 F        155 0.578
    ## 2 T        113 0.422
