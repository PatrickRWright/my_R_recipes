Using the map function with tilde (~) notations
-----------------------------------------------

In part inspired by
(this)\[<a href="https://towardsdatascience.com/functional-programming-in-r-with-purrr-469e597d0229" class="uri">https://towardsdatascience.com/functional-programming-in-r-with-purrr-469e597d0229</a>\].

    # %>%
    library(magrittr)
    # map
    library(purrr)

    ## 
    ## Attaching package: 'purrr'

    ## The following object is masked from 'package:magrittr':
    ## 
    ##     set_names

Map will apply a given function on each element of the input. The dot
(.) indicated the object passed through by the pipe. In this case first
the complete iris dataset and then a list of three data frames. One for
each species in the iris dataset.

    # split iris into a list by the different species and get the means for Sepal.Length and Sepal.Width
    iris %>%
      split(.$Species) %>%
      map(~ c(mean(.$Sepal.Length),
              mean(.$Sepal.Width)))

    ## $setosa
    ## [1] 5.006 3.428
    ## 
    ## $versicolor
    ## [1] 5.936 2.770
    ## 
    ## $virginica
    ## [1] 6.588 2.974

The **tilde (~)** statement in the **map** call is equivalent to the
following individual function and can thus replace the overhead of
predefining a function.

    means_sepal_iris <- function(data) {
        return(c(mean(data$Sepal.Length),
                 mean(data$Sepal.Width)))
    }

    iris %>%
      split(.$Species) %>%
      map(means_sepal_iris)

    ## $setosa
    ## [1] 5.006 3.428
    ## 
    ## $versicolor
    ## [1] 5.936 2.770
    ## 
    ## $virginica
    ## [1] 6.588 2.974

    iris %>%
      split(.$Species) %>%
      map(function(.) { return(c(mean(.$Sepal.Length),
                                 mean(.$Sepal.Width)))}
         )

    ## $setosa
    ## [1] 5.006 3.428
    ## 
    ## $versicolor
    ## [1] 5.936 2.770
    ## 
    ## $virginica
    ## [1] 6.588 2.974

    # Some sanity checks
    mean(iris$Sepal.Length[which(iris$Species == "setosa")]) == 5.006

    ## [1] TRUE

    mean(iris$Sepal.Length[which(iris$Species == "versicolor")]) == 5.936

    ## [1] TRUE

    mean(iris$Sepal.Width[which(iris$Species == "virginica")]) == 2.974

    ## [1] TRUE

    sessionInfo()

    ## R version 3.6.3 (2020-02-29)
    ## Platform: x86_64-pc-linux-gnu (64-bit)
    ## Running under: Ubuntu 18.04.5 LTS
    ## 
    ## Matrix products: default
    ## BLAS:   /usr/lib/x86_64-linux-gnu/blas/libblas.so.3.7.1
    ## LAPACK: /usr/lib/x86_64-linux-gnu/lapack/liblapack.so.3.7.1
    ## 
    ## locale:
    ##  [1] LC_CTYPE=C.UTF-8       LC_NUMERIC=C           LC_TIME=C.UTF-8       
    ##  [4] LC_COLLATE=C.UTF-8     LC_MONETARY=C.UTF-8    LC_MESSAGES=C.UTF-8   
    ##  [7] LC_PAPER=C.UTF-8       LC_NAME=C              LC_ADDRESS=C          
    ## [10] LC_TELEPHONE=C         LC_MEASUREMENT=C.UTF-8 LC_IDENTIFICATION=C   
    ## 
    ## attached base packages:
    ## [1] stats     graphics  grDevices utils     datasets  methods   base     
    ## 
    ## other attached packages:
    ## [1] purrr_0.3.3  magrittr_1.5
    ## 
    ## loaded via a namespace (and not attached):
    ##  [1] compiler_3.6.3  tools_3.6.3     htmltools_0.4.0 yaml_2.2.0     
    ##  [5] Rcpp_1.0.3      stringi_1.4.3   rmarkdown_2.1   knitr_1.25     
    ##  [9] stringr_1.4.0   xfun_0.10       digest_0.6.22   rlang_0.4.6    
    ## [13] evaluate_0.14
