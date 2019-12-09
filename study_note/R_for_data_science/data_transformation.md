## Study Note: R for Data Science

### Data Transformation

大多數時候資料的型態或者格式並不是最適合用以分析或是視覺化的情況，這個時候就必須進行資料轉換，以把資料轉變成方便分析者應用的形式。要進行 data transformation 之前，必須先對資料中每一個欄位的格式有基本的瞭解，才能夠進一步處理，故以下說明幾種基本的資料型態。

* ```int```代表整數。
* ```dbl```代表實數。
* ```chr```代表字串。
* ```dttm```代表時間資料，特別是日期加上時間的組合。
* ```lgl```代表儲存```TRUE```或者```FASLE```的資料。
* ```fctr```代表因子資料，用以儲存固定種類的值。
* ```date```代表日期的時間資料。

本書介紹用來進行 data transformation 的 package 為```dplyr```，這個套件中最常用的函數為以下。

* ```filter()```，根據特定條件選取出觀察值。
* ```arrange()```，把資料根據某個變數重新排列順序。
* ```select()```，選出特定的變數。
* ```mutate()```，根據現有變數創造新的變數。
* ```summarise()```，把多個數值整合成單一的總結數值。
* ```group_by()```，把資料根據某個變數分組，以進行後續的運算。

以上這些函數（除了```group_by()```之外）的共同特徵包括：第一個參數為想要處理的 data frame，後續的參數說明特殊的行為或者條件，其中涉及的所有參數名稱都不需要加上冒號，最後是這些函數產生的結果都會是新的 data frame。

```filter```可以針對特定的條件選取出資料的子集，第二和之後的參數放入不同的條件。

```
filter(flights, month == 1, day == 1)
#> # A tibble: 842 x 19
#>    year month   day dep_time sched_dep_time dep_delay arr_time
#>   <int> <int> <int>    <int>          <int>     <dbl>    <int>
#> 1  2013     1     1      517            515         2      830
#> 2  2013     1     1      533            529         4      850
#> 3  2013     1     1      542            540         2      923
#> 4  2013     1     1      544            545        -1     1004
#> 5  2013     1     1      554            600        -6      812
#> 6  2013     1     1      554            558        -4      740
#> # … with 836 more rows, and 12 more variables: sched_arr_time <int>,
#> #   arr_delay <dbl>, carrier <chr>, flight <int>, tailnum <chr>,
#> #   origin <chr>, dest <chr>, air_time <dbl>, distance <dbl>, hour <dbl>,
#> #   minute <dbl>, time_hour <dttm>
```

以上的程式碼就是針對```flights```這個 data frame 進行篩選，```month == 1```為第一個條件，```day == 1```為第二個條件。若沒有將這行程式碼指派給特定的參數，執行後會直接呈現部分結果在下方，若想要儲存篩選後的結構，就必須使用```<-```指派給特定參數，舉例如下，將程式的結果存在名為```jan1```的變數當中。

```
jan1 <- filter(flights, month == 1, day == 1)
```

若想要同時做到呈現和儲存變數這兩件事情，只需要在整行程式碼以括號包起來即可，舉例如下。

```
(dec25 <- filter(flights, month == 12, day == 25))
#> # A tibble: 719 x 19
#>    year month   day dep_time sched_dep_time dep_delay arr_time
#>   <int> <int> <int>    <int>          <int>     <dbl>    <int>
#> 1  2013    12    25      456            500        -4      649
#> 2  2013    12    25      524            515         9      805
#> 3  2013    12    25      542            540         2      832
#> 4  2013    12    25      546            550        -4     1022
#> 5  2013    12    25      556            600        -4      730
#> 6  2013    12    25      557            600        -3      743
#> # … with 713 more rows, and 12 more variables: sched_arr_time <int>,
#> #   arr_delay <dbl>, carrier <chr>, flight <int>, tailnum <chr>,
#> #   origin <chr>, dest <chr>, air_time <dbl>, distance <dbl>, hour <dbl>,
#> #   minute <dbl>, time_hour <dttm>
```
