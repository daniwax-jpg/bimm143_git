# Class17


\#Section 1. Proportion of G/G in a population

``` r
MXL <- read.csv("MXL.csv")
head(MXL)
```

      Sample..Male.Female.Unknown. Genotype..forward.strand. Population.s. Father
    1                  NA19648 (F)                       A|A ALL, AMR, MXL      -
    2                  NA19649 (M)                       G|G ALL, AMR, MXL      -
    3                  NA19651 (F)                       A|A ALL, AMR, MXL      -
    4                  NA19652 (M)                       G|G ALL, AMR, MXL      -
    5                  NA19654 (F)                       G|G ALL, AMR, MXL      -
    6                  NA19655 (M)                       A|G ALL, AMR, MXL      -
      Mother
    1      -
    2      -
    3      -
    4      -
    5      -
    6      -

``` r
table(MXL$Genotype..forward.strand.)
```


    A|A A|G G|A G|G 
     22  21  12   9 

``` r
table(MXL$Genotype..forward.strand.)/nrow(MXL) *100
```


        A|A     A|G     G|A     G|G 
    34.3750 32.8125 18.7500 14.0625 

Looking at a diffferent proportion (Great Britain)

``` r
gbr <- read.csv("gbr.csv")
table(gbr$Genotype..forward.strand.)/nrow(gbr) *100
```


         A|A      A|G      G|A      G|G 
    25.27473 18.68132 26.37363 29.67033 

This variant that is associated with childhood asthm is more frequent in
the GBR population than the MXL population by around 2x.

``` r
genotype <- read.table("genotype.txt")
```

``` r
table(genotype$geno)
```


    A/A A/G G/G 
    108 233 121 

> Q13. The sample size for each genotype is as follows: 108 A/A, 233
> A/G, 121 G/G.

``` r
df <- genotype
```

``` r
df$geno <- factor(df$geno, levels = c("A/A", "A/G", "G/G"))
```

``` r
box <- boxplot(exp ~ geno, data = df,
                   main = "Expression by Genotype",
                   xlab = "Genotype",
                   ylab = "Expression")
```

![](Class17_files/figure-commonmark/unnamed-chunk-9-1.png)

``` r
box
```

    $stats
             [,1]     [,2]     [,3]
    [1,] 15.42908  7.07505  6.67482
    [2,] 26.95022 20.62572 16.90256
    [3,] 31.24847 25.06486 20.07363
    [4,] 35.95503 30.55183 24.45672
    [5,] 49.39612 42.75662 33.95602

    $n
    [1] 108 233 121

    $conf
             [,1]     [,2]     [,3]
    [1,] 29.87942 24.03742 18.98858
    [2,] 32.61753 26.09230 21.15868

    $out
    [1] 51.51787 50.16704 51.30170 11.39643 48.03410

    $group
    [1] 1 1 1 1 2

    $names
    [1] "A/A" "A/G" "G/G"

``` r
box$stats[3,]
```

    [1] 31.24847 25.06486 20.07363

The median values are 31.24, 25.06, and 20.07.

> Q14. The SNPs are associated with their ancestral variant G; the
> higher the expression of the SNP (the more copies of G), the lower the
> expression of the gene.
