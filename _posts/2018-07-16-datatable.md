---
layout: post
title: Introduction to data tables 
tags: [R, data.table]
#image: /img/hello_world.jpeg
---

If you're used to working in R I'm sure you know *data.frame* from base R, a structure used for storing data in tables. There is a library that, from my point of view, offers the most efficient implementation of the aggregation logic in R and the nicest and compressed syntax.

*data.table* objects behave in the same way as the base *data.frame*. However, there are some differences not only related to the syntax but also to the efficiency of the programming. [*data.table*](https://cran.r-project.org/web/packages/data.table/data.table.pdf) offers fast and memory efficient: file reader and writer, aggregations, updates, equi, non-equi, rolling, range and interval joins, in a short and flexible syntax,for faster development

If you look for data.table help in google you'll find a looot of super useful websites, most of them better than this one. I'm sorry about that but this post is mainly for me, a way to collect the data.table theory. 

This post has been written based on this [*data.table* cheat sheet](https://s3.amazonaws.com/assets.datacamp.com/blog_assets/datatable_Cheat_Sheet_R.pdf)



```R
> library(data.table)

```
Creation of a data.table
```R
> DT <- data.table(Shop=c(rep("A",5),rep("B",2),rep("C",5)),
                 Units=floor(runif(12, min=1, max=101)), 
                 Volume=c(1:8,NA,100,11,12)*10,
                 Mass=12:1*100, 
                 ClassID = c(rep("Premium",2),rep("LowCost",5),rep("Medium",2),rep("Premium",3)))
```

```R
    Shop Units Volume Mass ClassID
 1:    A    94     10 1200 Premium
 2:    A    36     20 1100 Premium
 3:    A    96     30 1000 LowCost
 4:    A     3     40  900 LowCost
 5:    A    19     50  800 LowCost
 6:    B    26     60  700 LowCost
 7:    B    25     70  600 LowCost
 8:    C    49     80  500  Medium
 9:    C    29     NA  400  Medium
10:    C    22   1000  300 Premium
11:    C    51    110  200 Premium
12:    C    58    120  100 Premium
```

Overview of the data.table:
```R
> dim(DT)
> names(DT)
> head(DT)
> tail(DT)
> str(DT)
```
## The set family: 
**Change columns names: setnames(DT,oldname,newname)**
```R
# Change name of a column
> setnames(DT,"Shop","ShopID")

# Change names of a few columns by name by position
> setnames(DT, c(2:3), paste0(c('a','b'), '_2'))

# Change names of a few columns by name
> setnames(DT,c("a_2","b_2"),c("Units","Volume"))
```
**Change columns order: setcolorder(DT,colneworder)**
```R
> setcolorder(DT,c("ShopID","ClassID","Volume","Units","Mass"))
```
**Change a specific value set(DT, row, column, new value)**
```R
> set(DT,9,3,320)
```

## Selection of rows using i

**Select a number of rows**
```R
> DT[1:5]
> DT[1:5,]
```

**Select all rows that have a specific value in  a column**
```R
> DT[ClassID == "LowCost"]
> DT[ClassID != "LowCost"]
```

**Select all rows that have value x or y in a column**
```R
> DT[ClassID %in% c("Premium", "LowCost")]
> DT[Volume > 100]
```
Once you know *data.table* you'll love it! Not only will you reduce the computing time in your programs, but also your programming time.


