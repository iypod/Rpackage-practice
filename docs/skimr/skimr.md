[skimr package](https://cran.r-project.org/web/packages/skimr/index.html)
================

-   [パッケージ概要、きっかけ、モチベーション](#パッケージ概要きっかけモチベーション)
-   [使うパッケージ](#使うパッケージ)
-   [本番](#本番)
    -   [おまじない](#おまじない)
    -   [セクシーなサマリー](#セクシーなサマリー)
    -   [セクシーなサマリー(ヒストグラム文字化け回避版)](#セクシーなサマリーヒストグラム文字化け回避版)
    -   [データフレームで表現したいとき](#データフレームで表現したいとき)
    -   [それはそうと、group\_byも通るらしい](#それはそうとgroup_byも通るらしい)
    -   [それでは最後にセクシーで文字化けしてないtidyなデータフレームのサマリーを作ってみましょう](#それでは最後にセクシーで文字化けしてないtidyなデータフレームのサマリーを作ってみましょう)
    -   [おまじない解除](#おまじない解除)
-   [感想](#感想)
-   [環境](#環境)

<https://cran.r-project.org/web/packages/skimr/index.html>

パッケージ概要、きっかけ、モチベーション
----------------------------------------

-   リッチなsummaryが作れる。
-   [twitterでみつけた](https://twitter.com/d4tagirl/status/949055803465588736)。
-   ヒストグラムがデータフレームのなかに表示されるのかっこいいからやってみたい。

使うパッケージ
--------------

``` r
library(skimr)
library(dplyr) # %>%とかselectとかを使いたいだけ
library(tidyr) # spreadをつかいたいだけ
```

本番
----

### おまじない

2018年1月5日現在、[ウィンドウズではヒストグラム部分に文字化けが起こるので、おまじないが必要とのこと](https://github.com/ropenscilabs/skimr/blob/master/README.md#limitations-of-current-version)。

``` r
Sys.setlocale("LC_CTYPE", "Chinese")
```

    ## [1] "Chinese (Simplified)_China.936"

### セクシーなサマリー

おまじないしても、hist列は文字化けしている…

``` r
iris %>%
  skimr::skim()
```

    ## Skim summary statistics
    ##  n obs: 150 
    ##  n variables: 5 
    ## 
    ## Variable type: factor 
    ##   variable missing complete   n n_unique                       top_counts
    ## 1  Species       0      150 150        3 set: 50, ver: 50, vir: 50, NA: 0
    ##   ordered
    ## 1   FALSE
    ## 
    ## Variable type: numeric 
    ##       variable missing complete   n mean   sd min p25 median p75 max
    ## 1 Petal.Length       0      150 150 3.76 1.77 1   1.6   4.35 5.1 6.9
    ## 2  Petal.Width       0      150 150 1.2  0.76 0.1 0.3   1.3  1.8 2.5
    ## 3 Sepal.Length       0      150 150 5.84 0.83 4.3 5.1   5.8  6.4 7.9
    ## 4  Sepal.Width       0      150 150 3.06 0.44 2   2.8   3    3.3 4.4
    ##       hist
    ## 1 ｨ~ｨxｨxｨyｨ|ｨ|ｨzｨx
    ## 2 ｨ~ｨxｨxｨ|ｨzｨzｨyｨy
    ## 3 ｨyｨ~ｨ|ｨ~ｨ}ｨ|ｨyｨy
    ## 4 ｨxｨyｨ|ｨ~ｨzｨyｨxｨx

なお、baseのサマリーはこちらです。セクシーじゃないし、tidyでもないよね。

``` r
iris %>% 
  summary()
```

    ##   Sepal.Length    Sepal.Width     Petal.Length    Petal.Width   
    ##  Min.   :4.300   Min.   :2.000   Min.   :1.000   Min.   :0.100  
    ##  1st Qu.:5.100   1st Qu.:2.800   1st Qu.:1.600   1st Qu.:0.300  
    ##  Median :5.800   Median :3.000   Median :4.350   Median :1.300  
    ##  Mean   :5.843   Mean   :3.057   Mean   :3.758   Mean   :1.199  
    ##  3rd Qu.:6.400   3rd Qu.:3.300   3rd Qu.:5.100   3rd Qu.:1.800  
    ##  Max.   :7.900   Max.   :4.400   Max.   :6.900   Max.   :2.500  
    ##        Species  
    ##  setosa    :50  
    ##  versicolor:50  
    ##  virginica :50  
    ##                 
    ##                 
    ## 

### セクシーなサマリー(ヒストグラム文字化け回避版)

2018年1月5日現在、[markdownをknitするときは、文字化けがおこる、とのこと](https://github.com/ropenscilabs/skimr/blob/master/README.md#printing-spark-histograms-and-line-graphs-in-knitted-documents)。kable()で回避できるそう。だが、おしゃれさ(?)はちょっと減る、縦持ちのデータフレームっぽい見た目になる、 level列が追加される、value列とformatted列は実質的に同じもの、etc

``` r
iris %>%
  skimr::skim() %>% 
  head(11) %>% 
  kable() # knitrパッケージをロードしなくても、skimrパッケージから呼び出せる
```

| variable     | type    | stat     | level |        value| formatted |
|:-------------|:--------|:---------|:------|------------:|:----------|
| Sepal.Length | numeric | missing  | .all  |    0.0000000| 0         |
| Sepal.Length | numeric | complete | .all  |  150.0000000| 150       |
| Sepal.Length | numeric | n        | .all  |  150.0000000| 150       |
| Sepal.Length | numeric | mean     | .all  |    5.8433333| 5.84      |
| Sepal.Length | numeric | sd       | .all  |    0.8280661| 0.83      |
| Sepal.Length | numeric | min      | .all  |    4.3000000| 4.3       |
| Sepal.Length | numeric | p25      | .all  |    5.1000000| 5.1       |
| Sepal.Length | numeric | median   | .all  |    5.8000000| 5.8       |
| Sepal.Length | numeric | p75      | .all  |    6.4000000| 6.4       |
| Sepal.Length | numeric | max      | .all  |    7.9000000| 7.9       |
| Sepal.Length | numeric | hist     | .all  |           NA| ▂▇▅▇▆▅▂▂  |

### データフレームで表現したいとき

skimしたら、そのままdplyrの関数群に放り込むと、縦持ちのデータフレームっぽい見た目になる。

``` r
iris %>%
  skim() %>%
  arrange(variable) %>% 
  head(11)
```

    ## # A tibble: 11 x 6
    ##    variable     type    stat     level  value formatted
    ##    <chr>        <chr>   <chr>    <chr>  <dbl> <chr>    
    ##  1 Petal.Length numeric missing  .all    0    0        
    ##  2 Petal.Length numeric complete .all  150    150      
    ##  3 Petal.Length numeric n        .all  150    150      
    ##  4 Petal.Length numeric mean     .all    3.76 3.76     
    ##  5 Petal.Length numeric sd       .all    1.77 1.77     
    ##  6 Petal.Length numeric min      .all    1.00 1        
    ##  7 Petal.Length numeric p25      .all    1.60 1.6      
    ##  8 Petal.Length numeric median   .all    4.35 4.35     
    ##  9 Petal.Length numeric p75      .all    5.10 5.1      
    ## 10 Petal.Length numeric max      .all    6.90 6.9      
    ## 11 Petal.Length numeric hist     .all   NA    ｨ~ｨxｨxｨyｨ|ｨ|ｨzｨx

クラスも若干異なる様子。

``` r
iris %>%
  skim() %>%
  class()
```

    ## [1] "skim_df"    "tbl_df"     "tbl"        "data.frame"

``` r
iris %>%
  skim() %>%
  arrange(variable) %>% 
  class()
```

    ## [1] "tbl_df"     "tbl"        "data.frame"

### それはそうと、group\_byも通るらしい

``` r
iris %>%
  group_by(Species) %>% 
  skim() %>% 
  head(11) %>% 
  kable()
```

| Species | variable     | type    | stat     | level |       value| formatted |
|:--------|:-------------|:--------|:---------|:------|-----------:|:----------|
| setosa  | Sepal.Length | numeric | missing  | .all  |   0.0000000| 0         |
| setosa  | Sepal.Length | numeric | complete | .all  |  50.0000000| 50        |
| setosa  | Sepal.Length | numeric | n        | .all  |  50.0000000| 50        |
| setosa  | Sepal.Length | numeric | mean     | .all  |   5.0060000| 5.01      |
| setosa  | Sepal.Length | numeric | sd       | .all  |   0.3524897| 0.35      |
| setosa  | Sepal.Length | numeric | min      | .all  |   4.3000000| 4.3       |
| setosa  | Sepal.Length | numeric | p25      | .all  |   4.8000000| 4.8       |
| setosa  | Sepal.Length | numeric | median   | .all  |   5.0000000| 5         |
| setosa  | Sepal.Length | numeric | p75      | .all  |   5.2000000| 5.2       |
| setosa  | Sepal.Length | numeric | max      | .all  |   5.8000000| 5.8       |
| setosa  | Sepal.Length | numeric | hist     | .all  |          NA| ▂▃▅▇▇▃▁▂  |

### それでは最後にセクシーで文字化けしてないtidyなデータフレームのサマリーを作ってみましょう

``` r
iris %>%
  group_by(Species) %>% 
  skim() %>%
  select(-value, -level) %>% 
  spread(stat, formatted) %>%
  select(Species, variable, type, min, p25, mean, median, p75, max, sd, hist, everything()) %>% 
  arrange(variable) %>% 
  kable()
```

| Species    | variable     | type    | min | p25  | mean | median | p75  | max | sd   | hist     | complete | missing | n   |
|:-----------|:-------------|:--------|:----|:-----|:-----|:-------|:-----|:----|:-----|:---------|:---------|:--------|:----|
| setosa     | Petal.Length | numeric | 1   | 1.4  | 1.46 | 1.5    | 1.58 | 1.9 | 0.17 | ▁▁▅▇▇▅▂▁ | 50       | 0       | 50  |
| versicolor | Petal.Length | numeric | 3   | 4    | 4.26 | 4.35   | 4.6  | 5.1 | 0.47 | ▁▃▂▆▆▇▇▃ | 50       | 0       | 50  |
| virginica  | Petal.Length | numeric | 4.5 | 5.1  | 5.55 | 5.55   | 5.88 | 6.9 | 0.55 | ▂▇▃▇▅▂▁▂ | 50       | 0       | 50  |
| setosa     | Petal.Width  | numeric | 0.1 | 0.2  | 0.25 | 0.2    | 0.3  | 0.6 | 0.11 | ▂▇▁▂▂▁▁▁ | 50       | 0       | 50  |
| versicolor | Petal.Width  | numeric | 1   | 1.2  | 1.33 | 1.3    | 1.5  | 1.8 | 0.2  | ▆▃▇▅▆▂▁▁ | 50       | 0       | 50  |
| virginica  | Petal.Width  | numeric | 1.4 | 1.8  | 2.03 | 2      | 2.3  | 2.5 | 0.27 | ▂▁▇▃▃▆▅▃ | 50       | 0       | 50  |
| setosa     | Sepal.Length | numeric | 4.3 | 4.8  | 5.01 | 5      | 5.2  | 5.8 | 0.35 | ▂▃▅▇▇▃▁▂ | 50       | 0       | 50  |
| versicolor | Sepal.Length | numeric | 4.9 | 5.6  | 5.94 | 5.9    | 6.3  | 7   | 0.52 | ▃▂▇▇▇▃▅▂ | 50       | 0       | 50  |
| virginica  | Sepal.Length | numeric | 4.9 | 6.23 | 6.59 | 6.5    | 6.9  | 7.9 | 0.64 | ▁▁▃▇▅▃▂▃ | 50       | 0       | 50  |
| setosa     | Sepal.Width  | numeric | 2.3 | 3.2  | 3.43 | 3.4    | 3.68 | 4.4 | 0.38 | ▁▁▃▅▇▃▂▁ | 50       | 0       | 50  |
| versicolor | Sepal.Width  | numeric | 2   | 2.52 | 2.77 | 2.8    | 3    | 3.4 | 0.31 | ▁▂▃▅▃▇▃▁ | 50       | 0       | 50  |
| virginica  | Sepal.Width  | numeric | 2.2 | 2.8  | 2.97 | 3      | 3.18 | 3.8 | 0.32 | ▁▃▇▇▅▃▁▂ | 50       | 0       | 50  |

### おまじない解除

``` r
Sys.setlocale("LC_CTYPE", "Japanese")
```

    ## [1] "Japanese_Japan.932"

感想
----

ただ、ヒストグラムがデータフレームのなかに表示されるのがかっこよかっただけです。

環境
----

必要ないんだけど、なんとなくやってみたかった。

``` r
devtools::session_info()
```

    ## Session info -------------------------------------------------------------

    ##  setting  value                       
    ##  version  R version 3.4.2 (2017-09-28)
    ##  system   x86_64, mingw32             
    ##  ui       RTerm                       
    ##  language en                          
    ##  collate  Japanese_Japan.932          
    ##  tz       Asia/Tokyo                  
    ##  date     2018-01-06

    ## Packages -----------------------------------------------------------------

    ##  package       * version date       source        
    ##  assertthat      0.2.0   2017-04-11 CRAN (R 3.4.2)
    ##  backports       1.1.2   2017-12-13 CRAN (R 3.4.3)
    ##  base          * 3.4.2   2017-10-20 local         
    ##  bindr           0.1     2016-11-13 CRAN (R 3.4.2)
    ##  bindrcpp      * 0.2     2017-06-17 CRAN (R 3.4.2)
    ##  cli             1.0.0   2017-11-05 CRAN (R 3.4.3)
    ##  compiler        3.4.2   2017-10-20 local         
    ##  crayon          1.3.4   2017-09-16 CRAN (R 3.4.3)
    ##  datasets      * 3.4.2   2017-10-20 local         
    ##  devtools        1.13.4  2017-11-09 CRAN (R 3.4.3)
    ##  digest          0.6.13  2017-12-14 CRAN (R 3.4.3)
    ##  dplyr         * 0.7.4   2017-09-28 CRAN (R 3.4.2)
    ##  evaluate        0.10.1  2017-06-24 CRAN (R 3.4.2)
    ##  glue            1.2.0   2017-10-29 CRAN (R 3.4.3)
    ##  graphics      * 3.4.2   2017-10-20 local         
    ##  grDevices     * 3.4.2   2017-10-20 local         
    ##  highr           0.6     2016-05-09 CRAN (R 3.4.2)
    ##  htmltools       0.3.6   2017-04-28 CRAN (R 3.4.2)
    ##  knitr           1.18    2017-12-27 CRAN (R 3.4.3)
    ##  magrittr        1.5     2014-11-22 CRAN (R 3.4.2)
    ##  memoise         1.1.0   2017-04-21 CRAN (R 3.4.2)
    ##  methods       * 3.4.2   2017-10-20 local         
    ##  pander          0.6.1   2017-08-06 CRAN (R 3.4.3)
    ##  pillar          1.0.1   2017-11-27 CRAN (R 3.4.3)
    ##  pkgconfig       2.0.1   2017-03-21 CRAN (R 3.4.2)
    ##  purrr           0.2.4   2017-10-18 CRAN (R 3.4.3)
    ##  R6              2.2.2   2017-06-17 CRAN (R 3.4.3)
    ##  Rcpp            0.12.14 2017-11-23 CRAN (R 3.4.3)
    ##  RevoUtils     * 10.0.6  2017-10-17 local         
    ##  RevoUtilsMath * 10.0.1  2017-09-19 local         
    ##  rlang           0.1.6   2017-12-21 CRAN (R 3.4.3)
    ##  rmarkdown       1.8     2017-11-17 CRAN (R 3.4.3)
    ##  rprojroot       1.3-2   2018-01-03 CRAN (R 3.4.3)
    ##  skimr         * 1.0     2017-12-21 CRAN (R 3.4.3)
    ##  stats         * 3.4.2   2017-10-20 local         
    ##  stringi         1.1.6   2017-11-17 CRAN (R 3.4.2)
    ##  stringr         1.2.0   2017-02-18 CRAN (R 3.4.2)
    ##  tibble          1.4.1   2017-12-25 CRAN (R 3.4.3)
    ##  tidyr         * 0.7.2   2017-10-16 CRAN (R 3.4.3)
    ##  tidyselect      0.2.3   2017-11-06 CRAN (R 3.4.3)
    ##  tools           3.4.2   2017-10-20 local         
    ##  utf8            1.1.3   2018-01-03 CRAN (R 3.4.3)
    ##  utils         * 3.4.2   2017-10-20 local         
    ##  withr           2.1.1   2017-12-19 CRAN (R 3.4.3)
    ##  yaml            2.1.16  2017-12-12 CRAN (R 3.4.3)
