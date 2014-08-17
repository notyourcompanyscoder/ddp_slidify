---
title       : Manual Transmission is Better for Fuel Economy
subtitle    : A mini research report at mtcars dataset, devdataprods Course Project
framework   : io2012        # {io2012, html5slides, shower, dzslides, ...}
highlighter : highlight.js  # {highlight.js, prettify, highlight}
hitheme     : tomorrow      # 
mode        : selfcontained # {standalone, draft}
---
  
### Research Logistics  
- This research tries to find out the automaticity of vehicles' influence to fuel economy.
- Find a dataset includes the gauge of fuel economy, Miles per Gallon(`mpg`), and the automaticity of transmissions(`am`). The dataset is `mtcars`, extracted from 1974 *Motor Trend* US magazine, integrated in R `datasets` package.   

```r
data(mtcars)
```
- Firstly do some exploratory analysis. Plot the boxplot of `mpg` of **two groups** of samples, grouped by automaticity(`am`).
- Secondly make a series of deep analysis into the dataset. We will test the difference of the two grouped means against the `am` variable.
- Finally draw to the conclusion.

---
  
### Exploratory Data Analysis: The Boxplot




```r
qplot(factor(am), mpg, data = mtcars, geom = "boxplot", xlab = "Automaticity, 0 = AT, 1 = MT", ylab = "Miles Per Gallon")
```

![plot of chunk unnamed-chunk-3](assets/fig/unnamed-chunk-3.png) 

These plot intuitively shows that the automaticity of transmissions does influences the fuel economy of the vehicles listed on the `mtcars` dataset.

---  
      
### Deep Analysis: One-Way ANOVA  
  
ANOVA is used to analyze a factor's(`am`) influence towards the grouped outputs(`mpg`). We calculate the MSA and MSE of the dataset, then divide MSA by MSE and get the F ratio. Use the F test, if the null hypothesis is rejected, we can draw to the conclusion that the factor is significant. ANOVA is based on the assumption of homogeneity of variances. Let's test it first.  
**Test Methods: Bartlett Test of Homogeneity of Variances** (alpha = 0.05)

```r
## The P-value is:
bartlett.test(mpg ~ am, data = mtcars)$p.value
```

```
## [1] 0.07248
```
P-Value larger than alpha, thus we accept the null hypothesis that the two grouped variance are similar.  
Then we can do the main part of the analysis: One-Way ANOVA.

---
        
### Deep Analysis: One-Way ANOVA, and Conclusions
              


```r
summary(aov(mpg ~ am, data = mtcars))
```

```
##             Df Sum Sq Mean Sq F value  Pr(>F)    
## am           1    405     405    16.9 0.00029 ***
## Residuals   30    721      24                    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
```
The p-value is significantly small, null hypothesis rejected. 
Thus we will draw to the conclusion that the variable `am` influences the mean of different cars' MPG.  

BTW, the **mean difference** of `mpg` of the grouped models is:

```r
mean(subset(mtcars$mpg, mtcars$am == 1)) - mean(subset(mtcars$mpg, mtcars$am == 0))
```

```
## [1] 7.245
```

