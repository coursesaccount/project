Practical Machine Learning Course Project
=======================




### Load libraries



```r
> library(caret)
```


```r
> library(rpart)
```


### Load data



```r
> training=read.csv("~/Desktop/pml-training.csv")
```


```r
> testing=read.csv("~/Desktop/pml-testing.csv")
```


### Getting to know the data. 
### First how many variables and samples there are. 



```r
> dim(training)
```

```
[1] 19622   160
```


```r
> dim(testing)
```

```
[1]  20 160
```
### A lot of samples and variables - demands lots of computing power

### Find out the names of the variables to understand the problem

```r
> names(training) # var names
```

```
  [1] "X"                        "user_name"               
  [3] "raw_timestamp_part_1"     "raw_timestamp_part_2"    
  [5] "cvtd_timestamp"           "new_window"              
  [7] "num_window"               "roll_belt"               
  [9] "pitch_belt"               "yaw_belt"                
 [11] "total_accel_belt"         "kurtosis_roll_belt"      
 [13] "kurtosis_picth_belt"      "kurtosis_yaw_belt"       
 [15] "skewness_roll_belt"       "skewness_roll_belt.1"    
 [17] "skewness_yaw_belt"        "max_roll_belt"           
 [19] "max_picth_belt"           "max_yaw_belt"            
 [21] "min_roll_belt"            "min_pitch_belt"          
 [23] "min_yaw_belt"             "amplitude_roll_belt"     
 [25] "amplitude_pitch_belt"     "amplitude_yaw_belt"      
 [27] "var_total_accel_belt"     "avg_roll_belt"           
 [29] "stddev_roll_belt"         "var_roll_belt"           
 [31] "avg_pitch_belt"           "stddev_pitch_belt"       
 [33] "var_pitch_belt"           "avg_yaw_belt"            
 [35] "stddev_yaw_belt"          "var_yaw_belt"            
 [37] "gyros_belt_x"             "gyros_belt_y"            
 [39] "gyros_belt_z"             "accel_belt_x"            
 [41] "accel_belt_y"             "accel_belt_z"            
 [43] "magnet_belt_x"            "magnet_belt_y"           
 [45] "magnet_belt_z"            "roll_arm"                
 [47] "pitch_arm"                "yaw_arm"                 
 [49] "total_accel_arm"          "var_accel_arm"           
 [51] "avg_roll_arm"             "stddev_roll_arm"         
 [53] "var_roll_arm"             "avg_pitch_arm"           
 [55] "stddev_pitch_arm"         "var_pitch_arm"           
 [57] "avg_yaw_arm"              "stddev_yaw_arm"          
 [59] "var_yaw_arm"              "gyros_arm_x"             
 [61] "gyros_arm_y"              "gyros_arm_z"             
 [63] "accel_arm_x"              "accel_arm_y"             
 [65] "accel_arm_z"              "magnet_arm_x"            
 [67] "magnet_arm_y"             "magnet_arm_z"            
 [69] "kurtosis_roll_arm"        "kurtosis_picth_arm"      
 [71] "kurtosis_yaw_arm"         "skewness_roll_arm"       
 [73] "skewness_pitch_arm"       "skewness_yaw_arm"        
 [75] "max_roll_arm"             "max_picth_arm"           
 [77] "max_yaw_arm"              "min_roll_arm"            
 [79] "min_pitch_arm"            "min_yaw_arm"             
 [81] "amplitude_roll_arm"       "amplitude_pitch_arm"     
 [83] "amplitude_yaw_arm"        "roll_dumbbell"           
 [85] "pitch_dumbbell"           "yaw_dumbbell"            
 [87] "kurtosis_roll_dumbbell"   "kurtosis_picth_dumbbell" 
 [89] "kurtosis_yaw_dumbbell"    "skewness_roll_dumbbell"  
 [91] "skewness_pitch_dumbbell"  "skewness_yaw_dumbbell"   
 [93] "max_roll_dumbbell"        "max_picth_dumbbell"      
 [95] "max_yaw_dumbbell"         "min_roll_dumbbell"       
 [97] "min_pitch_dumbbell"       "min_yaw_dumbbell"        
 [99] "amplitude_roll_dumbbell"  "amplitude_pitch_dumbbell"
[101] "amplitude_yaw_dumbbell"   "total_accel_dumbbell"    
[103] "var_accel_dumbbell"       "avg_roll_dumbbell"       
[105] "stddev_roll_dumbbell"     "var_roll_dumbbell"       
[107] "avg_pitch_dumbbell"       "stddev_pitch_dumbbell"   
[109] "var_pitch_dumbbell"       "avg_yaw_dumbbell"        
[111] "stddev_yaw_dumbbell"      "var_yaw_dumbbell"        
[113] "gyros_dumbbell_x"         "gyros_dumbbell_y"        
[115] "gyros_dumbbell_z"         "accel_dumbbell_x"        
[117] "accel_dumbbell_y"         "accel_dumbbell_z"        
[119] "magnet_dumbbell_x"        "magnet_dumbbell_y"       
[121] "magnet_dumbbell_z"        "roll_forearm"            
[123] "pitch_forearm"            "yaw_forearm"             
[125] "kurtosis_roll_forearm"    "kurtosis_picth_forearm"  
[127] "kurtosis_yaw_forearm"     "skewness_roll_forearm"   
[129] "skewness_pitch_forearm"   "skewness_yaw_forearm"    
[131] "max_roll_forearm"         "max_picth_forearm"       
[133] "max_yaw_forearm"          "min_roll_forearm"        
[135] "min_pitch_forearm"        "min_yaw_forearm"         
[137] "amplitude_roll_forearm"   "amplitude_pitch_forearm" 
[139] "amplitude_yaw_forearm"    "total_accel_forearm"     
[141] "var_accel_forearm"        "avg_roll_forearm"        
[143] "stddev_roll_forearm"      "var_roll_forearm"        
[145] "avg_pitch_forearm"        "stddev_pitch_forearm"    
[147] "var_pitch_forearm"        "avg_yaw_forearm"         
[149] "stddev_yaw_forearm"       "var_yaw_forearm"         
[151] "gyros_forearm_x"          "gyros_forearm_y"         
[153] "gyros_forearm_z"          "accel_forearm_x"         
[155] "accel_forearm_y"          "accel_forearm_z"         
[157] "magnet_forearm_x"         "magnet_forearm_y"        
[159] "magnet_forearm_z"         "classe"                  
```

### Knowing the categories of the classe variable - to know what models should be used for prediction. 

```r
> levels(training$classe)
```

```
[1] "A" "B" "C" "D" "E"
```
### Since it is a qualitative variable with more than two categories, we can't use glm, or lm for example

### Showing the first 3 samples values to see how the data looks like



```r
> head(training, n=3L) # values
```

```
  X user_name raw_timestamp_part_1 raw_timestamp_part_2   cvtd_timestamp
1 1  carlitos           1323084231               788290 05/12/2011 11:23
2 2  carlitos           1323084231               808298 05/12/2011 11:23
3 3  carlitos           1323084231               820366 05/12/2011 11:23
  new_window num_window roll_belt pitch_belt yaw_belt total_accel_belt
1         no         11      1.41       8.07    -94.4                3
2         no         11      1.41       8.07    -94.4                3
3         no         11      1.42       8.07    -94.4                3
  kurtosis_roll_belt kurtosis_picth_belt kurtosis_yaw_belt
1                                                         
2                                                         
3                                                         
  skewness_roll_belt skewness_roll_belt.1 skewness_yaw_belt max_roll_belt
1                                                                      NA
2                                                                      NA
3                                                                      NA
  max_picth_belt max_yaw_belt min_roll_belt min_pitch_belt min_yaw_belt
1             NA                         NA             NA             
2             NA                         NA             NA             
3             NA                         NA             NA             
  amplitude_roll_belt amplitude_pitch_belt amplitude_yaw_belt
1                  NA                   NA                   
2                  NA                   NA                   
3                  NA                   NA                   
  var_total_accel_belt avg_roll_belt stddev_roll_belt var_roll_belt
1                   NA            NA               NA            NA
2                   NA            NA               NA            NA
3                   NA            NA               NA            NA
  avg_pitch_belt stddev_pitch_belt var_pitch_belt avg_yaw_belt
1             NA                NA             NA           NA
2             NA                NA             NA           NA
3             NA                NA             NA           NA
  stddev_yaw_belt var_yaw_belt gyros_belt_x gyros_belt_y gyros_belt_z
1              NA           NA         0.00            0        -0.02
2              NA           NA         0.02            0        -0.02
3              NA           NA         0.00            0        -0.02
  accel_belt_x accel_belt_y accel_belt_z magnet_belt_x magnet_belt_y
1          -21            4           22            -3           599
2          -22            4           22            -7           608
3          -20            5           23            -2           600
  magnet_belt_z roll_arm pitch_arm yaw_arm total_accel_arm var_accel_arm
1          -313     -128      22.5    -161              34            NA
2          -311     -128      22.5    -161              34            NA
3          -305     -128      22.5    -161              34            NA
  avg_roll_arm stddev_roll_arm var_roll_arm avg_pitch_arm stddev_pitch_arm
1           NA              NA           NA            NA               NA
2           NA              NA           NA            NA               NA
3           NA              NA           NA            NA               NA
  var_pitch_arm avg_yaw_arm stddev_yaw_arm var_yaw_arm gyros_arm_x
1            NA          NA             NA          NA        0.00
2            NA          NA             NA          NA        0.02
3            NA          NA             NA          NA        0.02
  gyros_arm_y gyros_arm_z accel_arm_x accel_arm_y accel_arm_z magnet_arm_x
1        0.00       -0.02        -288         109        -123         -368
2       -0.02       -0.02        -290         110        -125         -369
3       -0.02       -0.02        -289         110        -126         -368
  magnet_arm_y magnet_arm_z kurtosis_roll_arm kurtosis_picth_arm
1          337          516                                     
2          337          513                                     
3          344          513                                     
  kurtosis_yaw_arm skewness_roll_arm skewness_pitch_arm skewness_yaw_arm
1                                                                       
2                                                                       
3                                                                       
  max_roll_arm max_picth_arm max_yaw_arm min_roll_arm min_pitch_arm
1           NA            NA          NA           NA            NA
2           NA            NA          NA           NA            NA
3           NA            NA          NA           NA            NA
  min_yaw_arm amplitude_roll_arm amplitude_pitch_arm amplitude_yaw_arm
1          NA                 NA                  NA                NA
2          NA                 NA                  NA                NA
3          NA                 NA                  NA                NA
  roll_dumbbell pitch_dumbbell yaw_dumbbell kurtosis_roll_dumbbell
1         13.05         -70.49       -84.87                       
2         13.13         -70.64       -84.71                       
3         12.85         -70.28       -85.14                       
  kurtosis_picth_dumbbell kurtosis_yaw_dumbbell skewness_roll_dumbbell
1                                                                     
2                                                                     
3                                                                     
  skewness_pitch_dumbbell skewness_yaw_dumbbell max_roll_dumbbell
1                                                              NA
2                                                              NA
3                                                              NA
  max_picth_dumbbell max_yaw_dumbbell min_roll_dumbbell min_pitch_dumbbell
1                 NA                                 NA                 NA
2                 NA                                 NA                 NA
3                 NA                                 NA                 NA
  min_yaw_dumbbell amplitude_roll_dumbbell amplitude_pitch_dumbbell
1                                       NA                       NA
2                                       NA                       NA
3                                       NA                       NA
  amplitude_yaw_dumbbell total_accel_dumbbell var_accel_dumbbell
1                                          37                 NA
2                                          37                 NA
3                                          37                 NA
  avg_roll_dumbbell stddev_roll_dumbbell var_roll_dumbbell
1                NA                   NA                NA
2                NA                   NA                NA
3                NA                   NA                NA
  avg_pitch_dumbbell stddev_pitch_dumbbell var_pitch_dumbbell
1                 NA                    NA                 NA
2                 NA                    NA                 NA
3                 NA                    NA                 NA
  avg_yaw_dumbbell stddev_yaw_dumbbell var_yaw_dumbbell gyros_dumbbell_x
1               NA                  NA               NA                0
2               NA                  NA               NA                0
3               NA                  NA               NA                0
  gyros_dumbbell_y gyros_dumbbell_z accel_dumbbell_x accel_dumbbell_y
1            -0.02                0             -234               47
2            -0.02                0             -233               47
3            -0.02                0             -232               46
  accel_dumbbell_z magnet_dumbbell_x magnet_dumbbell_y magnet_dumbbell_z
1             -271              -559               293               -65
2             -269              -555               296               -64
3             -270              -561               298               -63
  roll_forearm pitch_forearm yaw_forearm kurtosis_roll_forearm
1         28.4         -63.9        -153                      
2         28.3         -63.9        -153                      
3         28.3         -63.9        -152                      
  kurtosis_picth_forearm kurtosis_yaw_forearm skewness_roll_forearm
1                                                                  
2                                                                  
3                                                                  
  skewness_pitch_forearm skewness_yaw_forearm max_roll_forearm
1                                                           NA
2                                                           NA
3                                                           NA
  max_picth_forearm max_yaw_forearm min_roll_forearm min_pitch_forearm
1                NA                               NA                NA
2                NA                               NA                NA
3                NA                               NA                NA
  min_yaw_forearm amplitude_roll_forearm amplitude_pitch_forearm
1                                     NA                      NA
2                                     NA                      NA
3                                     NA                      NA
  amplitude_yaw_forearm total_accel_forearm var_accel_forearm
1                                        36                NA
2                                        36                NA
3                                        36                NA
  avg_roll_forearm stddev_roll_forearm var_roll_forearm avg_pitch_forearm
1               NA                  NA               NA                NA
2               NA                  NA               NA                NA
3               NA                  NA               NA                NA
  stddev_pitch_forearm var_pitch_forearm avg_yaw_forearm
1                   NA                NA              NA
2                   NA                NA              NA
3                   NA                NA              NA
  stddev_yaw_forearm var_yaw_forearm gyros_forearm_x gyros_forearm_y
1                 NA              NA            0.03            0.00
2                 NA              NA            0.02            0.00
3                 NA              NA            0.03           -0.02
  gyros_forearm_z accel_forearm_x accel_forearm_y accel_forearm_z
1           -0.02             192             203            -215
2           -0.02             192             203            -216
3            0.00             196             204            -213
  magnet_forearm_x magnet_forearm_y magnet_forearm_z classe
1              -17              654              476      A
2              -18              661              473      A
3              -18              658              469      A
```
### The predictors are quantitative variables

### Counting the missing data. Not shown - since too long

```r
> # sapply(training, function(x)(sum(is.na(x)))) # NA counts
```

### Identifying variables with 0 missing values

```r
> nas <- sapply(training, function(x)(sum(is.na(x))))
```


```r
> nas[! nas %in% 19216]
```

```
                      X               user_name    raw_timestamp_part_1 
                      0                       0                       0 
   raw_timestamp_part_2          cvtd_timestamp              new_window 
                      0                       0                       0 
             num_window               roll_belt              pitch_belt 
                      0                       0                       0 
               yaw_belt        total_accel_belt      kurtosis_roll_belt 
                      0                       0                       0 
    kurtosis_picth_belt       kurtosis_yaw_belt      skewness_roll_belt 
                      0                       0                       0 
   skewness_roll_belt.1       skewness_yaw_belt            max_yaw_belt 
                      0                       0                       0 
           min_yaw_belt      amplitude_yaw_belt            gyros_belt_x 
                      0                       0                       0 
           gyros_belt_y            gyros_belt_z            accel_belt_x 
                      0                       0                       0 
           accel_belt_y            accel_belt_z           magnet_belt_x 
                      0                       0                       0 
          magnet_belt_y           magnet_belt_z                roll_arm 
                      0                       0                       0 
              pitch_arm                 yaw_arm         total_accel_arm 
                      0                       0                       0 
            gyros_arm_x             gyros_arm_y             gyros_arm_z 
                      0                       0                       0 
            accel_arm_x             accel_arm_y             accel_arm_z 
                      0                       0                       0 
           magnet_arm_x            magnet_arm_y            magnet_arm_z 
                      0                       0                       0 
      kurtosis_roll_arm      kurtosis_picth_arm        kurtosis_yaw_arm 
                      0                       0                       0 
      skewness_roll_arm      skewness_pitch_arm        skewness_yaw_arm 
                      0                       0                       0 
          roll_dumbbell          pitch_dumbbell            yaw_dumbbell 
                      0                       0                       0 
 kurtosis_roll_dumbbell kurtosis_picth_dumbbell   kurtosis_yaw_dumbbell 
                      0                       0                       0 
 skewness_roll_dumbbell skewness_pitch_dumbbell   skewness_yaw_dumbbell 
                      0                       0                       0 
       max_yaw_dumbbell        min_yaw_dumbbell  amplitude_yaw_dumbbell 
                      0                       0                       0 
   total_accel_dumbbell        gyros_dumbbell_x        gyros_dumbbell_y 
                      0                       0                       0 
       gyros_dumbbell_z        accel_dumbbell_x        accel_dumbbell_y 
                      0                       0                       0 
       accel_dumbbell_z       magnet_dumbbell_x       magnet_dumbbell_y 
                      0                       0                       0 
      magnet_dumbbell_z            roll_forearm           pitch_forearm 
                      0                       0                       0 
            yaw_forearm   kurtosis_roll_forearm  kurtosis_picth_forearm 
                      0                       0                       0 
   kurtosis_yaw_forearm   skewness_roll_forearm  skewness_pitch_forearm 
                      0                       0                       0 
   skewness_yaw_forearm         max_yaw_forearm         min_yaw_forearm 
                      0                       0                       0 
  amplitude_yaw_forearm     total_accel_forearm         gyros_forearm_x 
                      0                       0                       0 
        gyros_forearm_y         gyros_forearm_z         accel_forearm_x 
                      0                       0                       0 
        accel_forearm_y         accel_forearm_z        magnet_forearm_x 
                      0                       0                       0 
       magnet_forearm_y        magnet_forearm_z                  classe 
                      0                       0                       0 
```


### Having some ideea about data normality



```r
> library(e1071)
```


```r
> kurt <- sapply(training, function(x)(abs(kurtosis(x))))
```


```r
> length(kurt[kurt >1])
```

```
[1] 132
```


```r
> skew <- sapply(training, function(x)(abs(skewness(x))))
```


```r
> length(skew[skew >1])
```

```
[1] 117
```


### Lots of variables seem to be skewed or not normal. Thus linear models might but not necesarelly have problems. A nonlinear approach could be chosed.

### .
### Creating a subset of the training data with the variables of interest based on previous observations. 
### I excluded the variables with lots of missing data, date/time information, IDs, names...



```r
> training <- subset(training, select=c(classe, 
+ roll_arm, pitch_arm, yaw_arm, gyros_arm_x, gyros_arm_y, gyros_arm_z, accel_arm_x, accel_arm_y, accel_arm_z, magnet_arm_x, 
+   magnet_arm_y, magnet_arm_z, total_accel_arm,
+ roll_forearm, pitch_forearm, yaw_forearm, gyros_forearm_x, gyros_forearm_y, gyros_forearm_z, accel_forearm_x, accel_forearm_y, 
+   accel_forearm_z, magnet_forearm_x, magnet_forearm_y, magnet_forearm_z,total_accel_forearm,
+ roll_belt, pitch_belt, yaw_belt, gyros_belt_x, gyros_belt_y, gyros_belt_z, accel_belt_x, accel_belt_y, accel_belt_z, 
+   magnet_belt_x, magnet_belt_y, magnet_belt_z, total_accel_belt,
+ roll_dumbbell, pitch_dumbbell, yaw_dumbbell, gyros_dumbbell_x, gyros_dumbbell_y, gyros_dumbbell_z, accel_dumbbell_x, 
+   accel_dumbbell_y, accel_dumbbell_z, magnet_dumbbell_x, magnet_dumbbell_y, magnet_dumbbell_z, total_accel_dumbbell))
```


### Defining training control for K-fold crossvalidation with k=10 - a common used value (for a balance between bias and variation).



```r
> train_control<- trainControl(method="cv", number=10, savePredictions = TRUE)
```


### Training the model, using decision trees, since my computer is very slow. 
### When I tried other algorithms it was still working after letting it work for more than 20 minutes. 
### .
### The method works well even for nonlinear settings, is easy to interpret, and using crossvalidation it reduces the chances of overfitting. 
### True, the method is less accurate compared to bagging, random forests (but those are time consuming for my computer)
### .
### First setting the seed for reproductibility



```r
> set.seed(12345)
```
### Model training

```r
> model<- train(classe~., data=training, trControl=train_control, method="rpart")
```


### Printing the model accuracy 



```r
> model
```

```
CART 

19622 samples
   52 predictors
    5 classes: 'A', 'B', 'C', 'D', 'E' 

No pre-processing
Resampling: Cross-Validated (10 fold) 
Summary of sample sizes: 17660, 17659, 17659, 17660, 17661, 17661, ... 
Resampling results across tuning parameters:

  cp       Accuracy  Kappa 
  0.03568  0.5114    0.3621
  0.05999  0.4290    0.2301
  0.11515  0.3236    0.0597

Accuracy was used to select the optimal model using  the largest value.
The final value used for the model was cp = 0.03568. 
```


### The accuracy in the training set was 0.51. 
### I expect the testing (out of sample) accuracy to be lower, due to overfitting.

### .

### . Plotting the decision tree (on my computer I have some dependencies problems so I couldn't install the rattle package for better looking charts)


```r
> plot(model$finalModel, uniform=T)
```

<img src="figure/unnamed-chunk-24.png" title="plot of chunk unnamed-chunk-24" alt="plot of chunk unnamed-chunk-24" width="750" />


```r
> text(model$finalModel, use.n=T, all=T, cex=.8)
```

```
Error: plot.new has not been called yet
```
### The decision tree - text based

```r
> print(model$finalModel)
```

```
n= 19622 

node), split, n, loss, yval, (yprob)
      * denotes terminal node

 1) root 19622 14040 A (0.28 0.19 0.17 0.16 0.18)  
   2) roll_belt< 130.5 17977 12410 A (0.31 0.21 0.19 0.18 0.11)  
     4) pitch_forearm< -33.95 1578    10 A (0.99 0.0063 0 0 0) *
     5) pitch_forearm>=-33.95 16399 12400 A (0.24 0.23 0.21 0.2 0.12)  
      10) magnet_dumbbell_y< 439.5 13870  9953 A (0.28 0.18 0.24 0.19 0.11)  
        20) roll_forearm< 123.5 8643  5131 A (0.41 0.18 0.18 0.17 0.061) *
        21) roll_forearm>=123.5 5227  3500 C (0.077 0.18 0.33 0.23 0.18) *
      11) magnet_dumbbell_y>=439.5 2529  1243 B (0.032 0.51 0.043 0.22 0.19) *
   3) roll_belt>=130.5 1645    14 E (0.0085 0 0 0 0.99) *
```

### .
### Creating a subset of the testing data with the variables of interest (same ones used for training).



```r
> testing <- subset(testing, select=c(
+ roll_arm, pitch_arm, yaw_arm, gyros_arm_x, gyros_arm_y, gyros_arm_z, accel_arm_x, accel_arm_y, accel_arm_z, magnet_arm_x, 
+   magnet_arm_y, magnet_arm_z, total_accel_arm,
+ roll_forearm, pitch_forearm, yaw_forearm, gyros_forearm_x, gyros_forearm_y, gyros_forearm_z, accel_forearm_x, accel_forearm_y, 
+   accel_forearm_z, magnet_forearm_x, magnet_forearm_y, magnet_forearm_z,total_accel_forearm,
+ roll_belt, pitch_belt, yaw_belt, gyros_belt_x, gyros_belt_y, gyros_belt_z, accel_belt_x, accel_belt_y, accel_belt_z, 
+   magnet_belt_x, magnet_belt_y, magnet_belt_z, total_accel_belt,
+ roll_dumbbell, pitch_dumbbell, yaw_dumbbell, gyros_dumbbell_x, gyros_dumbbell_y, gyros_dumbbell_z, accel_dumbbell_x, 
+   accel_dumbbell_y, accel_dumbbell_z, magnet_dumbbell_x, magnet_dumbbell_y, magnet_dumbbell_z, total_accel_dumbbell))
```


### Predicting the classe variable using the testing data



```r
> predict(model, testing)
```

```
 [1] C A C A A C C A A A C C C A C A A A A C
Levels: A B C D E
```




