---
title: "ggplot2_分面(faceting)总结"
author: "zhuwengen"
date: "2020年5月27日"
output: html_document
---

```{r setup, include=FALSE}
knitr::opts_chunk$set(echo = TRUE)
```

ggplot2的分面有两种方式，分别使用 facet_wrap 或 facet_grid 函数。
主要参考: [ggplot2作图详解4：分面(faceting)](https://www.plob.org/article/7653.html).

```{r eval = TRUE }
# 准备工作
library(ggplot2)
set.seed(100)
d.sub <- diamonds[sample(x=nrow(diamonds), size=500), ]    #取500个数作为行
head(d.sub, 4) 
theme_set(theme_bw())
p <- ggplot(data=d.sub, aes(x=carat, y=price))

```

## 缠绕分面 facet_wrap
`facet_warp` 即“缠绕分面”，对数据分类只能应用一个标准，不同组数据获得的小形按"从左到右""从上到下"的“缠绕”顺序进行排列：

```{r eval = TRUE}
levels(d.sub$cut)

p  + geom_point() + facet_wrap(~cut)
```


**`facet_wrap`**用法
```{r eval=FALSE}
# 非运行代码
facet_wrap(facets, nrow=NULL, ncol=NULL, scales="fixed", shrink = TRUE, as.table = TRUE, drop = TRUE)

```

* facets: 分面参数~cut,表示用cut变量进行数据分类
* nrow：绘制图形的行数
* ncol：绘制图形的列数，一般nrow/ncol只设定一个即可
* `scales`：坐标刻度的范围，可以设定四种类型。fixed  表示所有小图均使用统一坐标范围；free表示每个小图按照各自数据范围自由调整坐标刻度范围；free_x为自由调整x轴刻度范围；free_y为自由调整y轴刻度范围。
* shrinks：也和坐标轴刻度有关，如果为TRUE（默认值）则按统计后的数据调整刻度范围，否则按统计前的数据设定坐标。
* as.table：和小图排列顺序有关的选项。如果为TRUE（默认）则按表格方式排列，即最大值（指分组level值）排在表格最后即右下角，否则排在左上角。
* drop：是否丢弃没有数据的分组，如果为TRUE（默认），则空数据组不绘图。


看看 scales 的设定效果：
```{r eval=FALSE }
p  + geom_point() + facet_wrap(~cut, scales="free") + 
     ggtitle('scales="free"')

p  + geom_point() + facet_wrap(~cut, scales="free_y") + 
     ggtitle('scales="free_y"')          

```

## 格网分面 facet_grid
格网分面可以应用多个标准对数据进行分组。还是先看看效果：
```{r eval=TRUE}
ggplot(data=diamonds, aes(x=carat, y=price))+
       geom_point()+
       facet_grid(color~cut)
```


**`facet_grid`**用法
```{r eval=FALSE}
# 非运行代码
facet_grid(facets, margins = FALSE, scales = "fixed", space = "fixed", shrink = TRUE,
           labeller = "label_value", as.table = TRUE, drop = TRUE)
```
和facet_wrap比较，除不用设置ncol和nrow外（facets公式已经包含）外还有几个参数不同：  
* `margins`: 这不是设定图形边界的参数。它是指用于分面的包含每个变量元素所有数据的数据组。     
* `space`:这个参数要配合`scales`使用，如果为fixed（默认），所有小图的大小都一样，如果为free/free_x/free_y，小图的大小将按照坐标轴的跨度比例进行设置。  


```{r eval=TRUE}
colnames(diamonds)
ggplot(data=diamonds, aes(x=carat, y=price, alpha=))+
       geom_point()+
       facet_grid(color~cut, space="free_x", scales="free_x")

```





