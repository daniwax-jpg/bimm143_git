# Class_07


Today we will explore some fundamental machine learning methods
including clustering and dimensionality reduction \## K-means clustering

To see how this works let’s first makeup some data to cluster where we
know what the answer should be. We can use the `rnorm()` function to
help here:

``` r
x <- c(rnorm(30, mean=-3), rnorm(30, mean=3))
y <- rev(x)
z <- cbind(x,y)
plot(z)
```

![](Class07_files/figure-commonmark/unnamed-chunk-1-1.png)

The function for K-means clustering in base R is `kmeans()`

``` r
k <- kmeans(z, centers=2)
k
```

    K-means clustering with 2 clusters of sizes 30, 30

    Cluster means:
              x         y
    1  2.981156 -2.793796
    2 -2.793796  2.981156

    Clustering vector:
     [1] 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 1 1 1 1 1 1 1 1
    [39] 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1

    Within cluster sum of squares by cluster:
    [1] 74.33361 74.33361
     (between_SS / total_SS =  87.1 %)

    Available components:

    [1] "cluster"      "centers"      "totss"        "withinss"     "tot.withinss"
    [6] "betweenss"    "size"         "iter"         "ifault"      

To get the results of the returned list object we can use the dollar `$`
syntax

> Q. How many points are in each cluster?

``` r
k$size
```

    [1] 30 30

> Q. What ‘component’ of your result object details - Cluster
> assignment/ membership? -Cluster center?

``` r
help("kmeans")
k$cluster
```

     [1] 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 1 1 1 1 1 1 1 1
    [39] 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1

``` r
k$centers
```

              x         y
    1  2.981156 -2.793796
    2 -2.793796  2.981156

Q. Make a clustering results figure of the data colored by cluster
membership and show cluster centers.

``` r
plot(z, col=k$cluster, pch=16)
points(k$centers, col="blue", pch=1, cex=3)
```

![](Class07_files/figure-commonmark/unnamed-chunk-6-1.png)

K-means clustering is very popular as it is very fast and relatively
straight forward: it takes numeric data as input and reutrns the cluster
membership vector etc.

The “issue” here is we tell `kmeans` how many clusters we want!

> Q. Run kmeans again and cluster into 4 grps/clusters and plot the
> results like we did above?

``` r
k4 <- kmeans(z, centers=4)
plot(z, col=k4$cluster, pch=16)
points(k4$centers, col="blue", pch=1, cex=3)
```

![](Class07_files/figure-commonmark/unnamed-chunk-7-1.png)

``` r
k3 <- kmeans(z, centers=3)
k2 <- kmeans(z, centers=2)
k4 <- kmeans(z, centers=4)
k5 <- kmeans(z, centers=5)
k1 <- kmeans(z, centers=1)
```

``` r
z <-  c(k1$tot.withinss,
        k2$tot.withinss,
        k3$tot.withinss,
        k4$tot.withinss)
plot(z, type="b")
```

![](Class07_files/figure-commonmark/unnamed-chunk-9-1.png)

``` r
n <-  NULL
for (i in 1:5) {
  n <- c(n, kmeans(x, centers=i)$tot.withinss)
}
plot(n, typ="b")
```

![](Class07_files/figure-commonmark/unnamed-chunk-10-1.png)

\##Hierarchichal clustering

The main “base” R fubction for Hierarchichal CLusyering is called
`hclust()`. Here we can’t just input our data we need to first calculate
a distance matrix for our data (e.g. `dist()` for our data) and use this
as input to `hclust()`

``` r
d <-  dist(x)
hc <- hclust(d)
hc
```


    Call:
    hclust(d = d)

    Cluster method   : complete 
    Distance         : euclidean 
    Number of objects: 60 

There is a plot method for hc, let’s try it:

``` r
plot(hc)
abline(h=8, col="red")
```

![](Class07_files/figure-commonmark/unnamed-chunk-12-1.png)

To get out cluster “membership” vector (i.ei our main clustering result)
we can “cut” the tree at a given height or at a height that yields a
given “k” groups

``` r
cutree(hc, h=8)
```

     [1] 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 2 2 2 2 2 2 2 2
    [39] 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2

``` r
grps <- cutree(hc, k=2)
```

> Q. Plot the data with our hclust result coloring

``` r
plot(hc)
abline(h=8, col="red")
```

![](Class07_files/figure-commonmark/unnamed-chunk-15-1.png)

\#PRincipal Component Analysis(PCA)

\##PCA of UK Food Data

Import food data from an oline CSV file:

``` r
url <- "https://tinyurl.com/UK-foods"
x <- read.csv(url)
head(x)
```

                   X England Wales Scotland N.Ireland
    1         Cheese     105   103      103        66
    2  Carcass_meat      245   227      242       267
    3    Other_meat      685   803      750       586
    4           Fish     147   160      122        93
    5 Fats_and_oils      193   235      184       209
    6         Sugars     156   175      147       139

``` r
x <-  read.csv(url, row.names=1)
x
```

                        England Wales Scotland N.Ireland
    Cheese                  105   103      103        66
    Carcass_meat            245   227      242       267
    Other_meat              685   803      750       586
    Fish                    147   160      122        93
    Fats_and_oils           193   235      184       209
    Sugars                  156   175      147       139
    Fresh_potatoes          720   874      566      1033
    Fresh_Veg               253   265      171       143
    Other_Veg               488   570      418       355
    Processed_potatoes      198   203      220       187
    Processed_Veg           360   365      337       334
    Fresh_fruit            1102  1137      957       674
    Cereals                1472  1582     1462      1494
    Beverages                57    73       53        47
    Soft_drinks            1374  1256     1572      1506
    Alcoholic_drinks        375   475      458       135
    Confectionery            54    64       62        41

``` r
rownames(x) <-  x[,1]
x <-  x[,-1]
x
```

         Wales Scotland N.Ireland
    105    103      103        66
    245    227      242       267
    685    803      750       586
    147    160      122        93
    193    235      184       209
    156    175      147       139
    720    874      566      1033
    253    265      171       143
    488    570      418       355
    198    203      220       187
    360    365      337       334
    1102  1137      957       674
    1472  1582     1462      1494
    57      73       53        47
    1374  1256     1572      1506
    375    475      458       135
    54      64       62        41

``` r
barplot(as.matrix(x), beside=T, col=rainbow(nrow(x)))
```

![](Class07_files/figure-commonmark/unnamed-chunk-19-1.png)

``` r
pairs(x, col=rainbow(10), pch=16)
```

![](Class07_files/figure-commonmark/unnamed-chunk-19-2.png)

> Main point: it can be difficult to spot major trends and patterns even
> in relatively small multivariate datasets (here we only have 17
> dimensions, typically we have 1000s)

\##PCA to the rescue

The main function in “base” R for PCA is called `prcomp()`.

I will ake the transpose of our food data so the “foods” are in the
columns:

``` r
t(x)
```

              105 245 685 147 193 156  720 253 488 198 360 1102 1472 57 1374 375 54
    Wales     103 227 803 160 235 175  874 265 570 203 365 1137 1582 73 1256 475 64
    Scotland  103 242 750 122 184 147  566 171 418 220 337  957 1462 53 1572 458 62
    N.Ireland  66 267 586  93 209 139 1033 143 355 187 334  674 1494 47 1506 135 41

``` r
pca <-  prcomp(t(x))
summary(pca)
```

    Importance of components:
                                PC1      PC2       PC3
    Standard deviation     379.8991 260.5533 1.438e-13
    Proportion of Variance   0.6801   0.3199 0.000e+00
    Cumulative Proportion    0.6801   1.0000 1.000e+00

``` r
pca$x
```

                    PC1        PC2           PC3
    Wales     -288.9534  226.36855  1.776357e-15
    Scotland  -141.3603 -284.81172  4.298784e-13
    N.Ireland  430.3137   58.44317 -9.592327e-14

``` r
cols <-  c("orange", "red", "blue", "darkgreen")
plot(pca$x[,1], pca$x[,2], col=cols, pch=16)
```

![](Class07_files/figure-commonmark/unnamed-chunk-22-1.png)

``` r
library(ggplot2)

ggplot(pca$x, aes(PC1, PC2)) +
         geom_point()
```

![](Class07_files/figure-commonmark/unnamed-chunk-23-1.png)

PCA looks super useful and we will come back to describe this further
next day :-)
