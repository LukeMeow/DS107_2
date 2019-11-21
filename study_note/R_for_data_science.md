## Study Note: R for Data Science

### Data Visualization

本書介紹的資料視覺化工具是 R packages 中最經典的繪圖套件 ggplot2，ggplot2 在語法以及呈現上的特色為「分層」，每張圖表都是一層一層的元素或者函數堆疊而成。

首先使用```ggplot()```導入資料作為第一層，第二層透過 geom function 說明產生的圖表類型，例如 ```geom_point()```作散點圖。此外在 geom function 中```mapping``` 的參數放入```aes()```呈現座標系統。ggplot2 視覺化最基本的模版如下。

```
ggplot(data = <DATA>) +
  <GEOM_FUNCTION>(mapping = aes(<MAPPINGS>))
```

geom function 中```mapping ```參數的```aes()```除了放入```x```和```y```兩個變量來控制座標系統外， 還可以控制其他視覺特性來呈現不同的變量，像是```color```、```size```、```shape```、```alpha```等，例如```<GEOM_FUNCTION>(mapping = aes(x = …, y = …, color = …, size = …))```。

假使想要呈現單一的視覺特性，就必須將該特性的參數獨立於```mapping```之外，例如想要做出皆為藍色點的散點圖，語法為```geom_point(mapping = aes(<MAPPINGS>), color = “blue”)```。

一組資料中某變數的不同特性除了可以藉由視覺特性來呈現之外，也可以透過「子圖」來呈現，以解決有時視覺呈現過於複雜難解的問題。子圖的概念是跟舉某變項的類別，將資料分成數張圖共同呈現，語法是在視覺化物件上再加一層```facet_wrap( ~ …)```即可，若想要利用兩個變數的組合來呈現子圖，則是要加上```facet_grid(… ~ …)```。

ggplot2 在一張圖中可以共同呈現數個 geom function 產生的物件，只要疊加數層 geom function 即可。舉例如下。

```
ggplot(data = …) +
  geom_point(mapping = aes(x = …, y = …)) +
  geom_smooth(mapping = aes(x = …, y = …))
```

假使兩個 geom function 的呈現都是建立在相同的座標系統上的話，可以直接將```mapping```的參數放到```ggplot()```中作為全域的設定，其他針對部分 geom function 所做的改變（```color```、```alpha```等）再放入各自函數中更新即可。舉例如下

```
ggplot(data = …, mapping = aes(x = …, y = …))) +
  geom_point(mapping = aes(color = …)) +
  geom_smooth()
```

有些 geom function 呈現的過程會涉及統計轉換（statistical transformation），最常見的例子就是```geom_bar()```。在該函式中有一個參數為```stat```，預設值為”count”，代表在產生圖表的過程中，會根據```aes()```中```x```的變數去計算該變數中不同類別的count，並且作為```y```變數呈現，所以語法會呈現如下。

```
ggplot(data = …) +
  geom_bar(mapping = aes(x = …))
```

在以上的語法中，並不需要去說明```y```變項，該函數會根據```stat```的預設參數”count”計算不同```x```類別的個數並作為```y```變數呈現。假設想要自己主導```y```變項所對應的數值，只要將```stat```設定為”identity”，並記得放入```y```變項即可，語法呈現如下。

```
ggplot(data = …) + 
  geom_bar(mapping = aes(x = …, y = …), stat = “identity”)
```

如果想要讓 y 軸呈現各類 x 的百分比。

```
ggplot(data = …) + 
  geom_bar(mapping = aes(x = …, y = ..prop.., group = 1))
```

之所以要額外設定```group```為常數，是因為```geom_bar()```預設將資料根據```x```分組，並且個別計算各組的數量，然而當我們在計算百分比時，想要計算的是每一個類別和全部資料比較而得到的結果，如果沒有額外設定```group```為任一常數，會得到每一個類別都是百分之百的結果（電腦會理解為類別1佔類別1的比例為百分之百，以此類推）。
ggplot2也能夠有能夠呈現更多統計轉換的繪圖方式，使用的函數為```stat_summary()```，語法如下。

```
ggplot(data = …) +
  stat_summary(
  mapping = aes(x = …, y = …),
  fun.min = min,
  fun.max = max,
  fun.y = median
)
```
