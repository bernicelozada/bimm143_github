# Class 5: Data Visualization with ggplot
Bernice Lozada (PID: A16297973)

Today we will have our first play with the **ggplot2** package - one of
the most popular graphics packages on the planet.

There are many plotting systems in R. These include so-called *“base”*
plotting/graphics.

``` r
plot(cars)
```

![](class05_files/figure-commonmark/unnamed-chunk-1-1.png)

Base plot is generally rather short code and somewhat dull plots - but
it is always there for you and is fast for big datasets.

``` r
# install.packages("ggplot2")
library(ggplot2)

ggplot(cars)
```

![](class05_files/figure-commonmark/unnamed-chunk-2-1.png)

The command to install the package first using `install.packages()` in
the R console to make it permanent.

To use a package, it needs to be loaded up with a `library()` call.

Every ggplot has at least three things:

- **data** (the data.frame with the plotting data)
- **aes** (aesthetic mapping of data to plot)
- **geom** (how you want plot to look - points, lines, etc.)

``` r
bp <- ggplot(cars) +
  aes(x=speed, y= dist) +
  geom_point()
bp
```

![](class05_files/figure-commonmark/unnamed-chunk-3-1.png)

``` r
bp_new <- bp + geom_smooth(se = FALSE, method = "lm") +
  labs(title = "Stopping Distance of Old Cars",
       x = "Speed (MPH)",
       y = "Distance (ft)",
       caption = "From the cars dataset") +
  theme_bw()
bp_new
```

    `geom_smooth()` using formula = 'y ~ x'

![](class05_files/figure-commonmark/unnamed-chunk-4-1.png)

## A more complicated scatter plot

Here we make a plot of gene expression data:

``` r
url <- "https://bioboot.github.io/bimm143_S20/class-material/up_down_expression.txt"
genes <- read.delim(url)
head(genes)
```

            Gene Condition1 Condition2      State
    1      A4GNT -3.6808610 -3.4401355 unchanging
    2       AAAS  4.5479580  4.3864126 unchanging
    3      AASDH  3.7190695  3.4787276 unchanging
    4       AATF  5.0784720  5.0151916 unchanging
    5       AATK  0.4711421  0.5598642 unchanging
    6 AB015752.4 -3.6808610 -3.5921390 unchanging

``` r
nrow(genes)
```

    [1] 5196

``` r
colnames(genes)
```

    [1] "Gene"       "Condition1" "Condition2" "State"     

``` r
ncol(genes)
```

    [1] 4

``` r
table(genes$State)
```


          down unchanging         up 
            72       4997        127 

``` r
#fraction
round(sum(genes$State == "up")/nrow(genes)*100,2)
```

    [1] 2.44

``` r
n.gene <- nrow(genes)
n.up <- sum(genes$State == "up")

up.percent <- n.up/n.gene * 100
round(up.percent, 2)
```

    [1] 2.44

``` r
p <- ggplot(genes) + 
  aes(x = Condition1, y = Condition2, col = State) +
  geom_point()
```

``` r
# Changing Colors
p + scale_colour_manual(values = c("yellow","orange","pink")) +
  labs(x = "Control (no drug)", y = "Drug Treatment", title = "Gene Expression Changes Upon Drug Treatment") + 
  theme_bw()
```

![](class05_files/figure-commonmark/unnamed-chunk-8-1.png)

## Exploring the gapmider dataset

Load up the gapminder dataset for practice with different aes mappings.

> Find number of countries in databasse

``` r
url <- "https://raw.githubusercontent.com/jennybc/gapminder/master/inst/extdata/gapminder.tsv"

gapminder <- read.delim(url)

table(gapminder$continent)
```


      Africa Americas     Asia   Europe  Oceania 
         624      300      396      360       24 

``` r
# Can use unique() function
length(unique(gapminder$continent))
```

    [1] 5

``` r
# number of countries
length(unique(gapminder$country))
```

    [1] 142

``` r
library(dplyr)
```


    Attaching package: 'dplyr'

    The following objects are masked from 'package:stats':

        filter, lag

    The following objects are masked from 'package:base':

        intersect, setdiff, setequal, union

``` r
gapminder_2007 <- gapminder %>% filter(year==2007)

head(gapminder_2007)
```

          country continent year lifeExp      pop  gdpPercap
    1 Afghanistan      Asia 2007  43.828 31889923   974.5803
    2     Albania    Europe 2007  76.423  3600523  5937.0295
    3     Algeria    Africa 2007  72.301 33333216  6223.3675
    4      Angola    Africa 2007  42.731 12420476  4797.2313
    5   Argentina  Americas 2007  75.320 40301927 12779.3796
    6   Australia   Oceania 2007  81.235 20434176 34435.3674

``` r
ggplot(gapminder) + aes(x=gdpPercap, y = lifeExp, col=continent, size = pop) + geom_point(alpha=0.2)
```

![](class05_files/figure-commonmark/unnamed-chunk-11-1.png)

``` r
## for 2007
ggplot(gapminder_2007) + aes(x=gdpPercap, y = lifeExp, col=continent, size = pop) + geom_point(alpha=0.6)
```

![](class05_files/figure-commonmark/unnamed-chunk-11-2.png)

With dyplr

``` r
#install.packages("dplyr")
library(dplyr)
gapminder_2007 <- filter(gapminder, year == 2007)
head(gapminder_2007)
```

          country continent year lifeExp      pop  gdpPercap
    1 Afghanistan      Asia 2007  43.828 31889923   974.5803
    2     Albania    Europe 2007  76.423  3600523  5937.0295
    3     Algeria    Africa 2007  72.301 33333216  6223.3675
    4      Angola    Africa 2007  42.731 12420476  4797.2313
    5   Argentina  Americas 2007  75.320 40301927 12779.3796
    6   Australia   Oceania 2007  81.235 20434176 34435.3674

Plot of 2007 with population and continent data

    ::: {.cell}

    ```{.r .cell-code}
    ggplot(gapminder_2007) + aes(x=gdpPercap, y = lifeExp, col=continent, size = pop) + geom_point(alpha=0.6)

<div class="cell-output-display">

![](class05_files/figure-commonmark/unnamed-chunk-13-1.png)

</div>

:::

Facet_wrap data to compare 1957 and 2007

``` r
gapminder_1957 <- filter(gapminder, year == 1957 | year == 2007)
ggplot(gapminder_1957) + 
  geom_point(aes(x = gdpPercap, y = lifeExp, color=continent,
                 size = pop), alpha=0.7) + 
  scale_size_area(max_size = 10) +
  facet_wrap(~year)
```

![](class05_files/figure-commonmark/unnamed-chunk-14-1.png)
