## Study Note: R for Data Science

### Data Visualization

本書介紹的資料視覺化工具是 R packages 中最經典的繪圖套件 ggplot2，ggplot2 在語法以及呈現上的特色為「分層」，每張圖表都是一層一層的函數或者物件堆疊而成。

首先使用```ggplot()```導入資料作為第一層，第二層透過 geom function 說明產生的圖表類型，例如```geom_point()```產生散點圖。此外在 geom function 的參數```mapping```放入```aes()```呈現座標系統。ggplot2 視覺化最基本的模版如下。

```
ggplot(data = <DATA>) +
  <GEOM_FUNCTION>(mapping = aes(<MAPPINGS>))
```

參數```mapping ```中的```aes()```除了放入```x```和```y```來呈現座標系統外， 還可以控制其他視覺特性來呈現不同的變量，像是```color```、```size```、```shape```、```alpha```等，例如```<GEOM_FUNCTION>(mapping = aes(x = …, y = …, color = …, size = …))```。

若想要呈現統一的視覺特性，就必須將該特性的參數獨立於```mapping```之外，例如想要產生皆為藍色點的散點圖，語法為```geom_point(mapping = aes(<MAPPINGS>), color = “blue”)```。

資料的變數除了可以透過視覺特性來呈現之外，也可以利用「子圖」呈現，以解決有時視覺上過於複雜難辨的問題。子圖的概念是根據某變項的類別，將資料分成數張圖共同呈現，語法是在視覺化物件上再加一層```facet_wrap( ~ …)```即可，若想要利用兩個變數的組合呈現子圖，則要加上```facet_grid(… ~ …)```。

ggplot2 在可以共同呈現數個 geom function 產生的物件，只要疊加數層 geom function 即可，語法舉例如下。

```
ggplot(data = …) +
  geom_point(mapping = aes(x = …, y = …)) +
  geom_smooth(mapping = aes(x = …, y = …))
```

若兩個 geom function 擁有相同的座標系統，可以直接將參數```mapping```放到```ggplot()```中作為全域設定，其他針對部分 geom function 所做的改變（```color```、```alpha```等）只要放入各自的函數中更新即可，語法舉例如下。

```
ggplot(data = …, mapping = aes(x = …, y = …))) +
  geom_point(mapping = aes(color = …)) +
  geom_smooth()
```

有些 geom function 的呈現涉及統計轉換（statistical transformation），最常見的例子是```geom_bar()```。在該函式中有一個參數為```stat```，預設值為 ”count”，代表在產生圖表時，會根據```aes()```中的```x```去計算該變數中不同類別的 count，並且作為```y```呈現，所以語法呈現如下。

```
ggplot(data = …) +
  geom_bar(mapping = aes(x = …))
```

在以上的語法中，不需要說明```y```，該函數會根據```stat```的預設參數 ”count” 計算不同```x```的個數並作為```y```呈現。若想要主導```y```對應的數值，只要將參數```stat```設為 ”identity”，並說明```y```即可，語法舉例如下。

```
ggplot(data = …) + 
  geom_bar(mapping = aes(x = …, y = …), stat = “identity”)
```

若想要讓```y```呈現百分比，語法舉例如下。

```
ggplot(data = …) + 
  geom_bar(mapping = aes(x = …, y = ..prop.., group = 1))
```

之所以要額外設定參數```group```為常數，是因為```geom_bar()```預設將資料根據```x```分組，並且個別計算各組的數量，然而在計算百分比時，所求的是每個類別之於全部資料的佔比，如果沒有額外設定參數```group```為任一常數，會得到每一個類別都是百分之百的結果（電腦會理解為類別 1 佔類別 1 的比例為百分之百，以此類推）。

ggplot2 也能夠有能夠呈現更多統計轉換的繪圖方式，使用的函數為```stat_summary()```，語法舉例如下。

```
ggplot(data = …) +
  stat_summary(
  mapping = aes(x = …, y = …),
  fun.min = min,
  fun.max = max,
  fun.y = median
)
```
