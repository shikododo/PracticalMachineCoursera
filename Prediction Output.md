Practical Machine Learning - assignment writeup
========================================================

We will start by loading the caret library, and then reading all data:


```r
library(caret)
```

```
## Warning: package 'caret' was built under R version 2.15.3
```

```
## Loading required package: lattice
## Loading required package: ggplot2
```

```
## Warning: package 'ggplot2' was built under R version 2.15.3
```

```r
set.seed(33833)
data <- read.csv("pml-training.csv")
```


Some variables should be excluded because most of them are NA:

```r
remove <- apply(data, 2, function(x) sum(is.na(x)))
data <- data[, names(remove[remove < 1000])]
```

The total number of remaining variables is 93.
We will then divide this data into training and test sets with 60% of the data in the training set:

```r
inTrain <- createDataPartition(y = data$classe, p = 0.6, list = FALSE)
training <- data[inTrain, ]
testing <- data[-inTrain, ]
```


We will use Random Forests prediction alogorithm on the training data set:

```r
modelFit <- train(classe ~ ., data = training, method = "rf", prox = TRUE)
```

```
## Error: cannot allocate vector of size 618.6 Mb
```

```r
modelFit
```

```
## Error: object 'modelFit' not found
```


To test out of sample error we need to predict our model in a new data set (the remaining 40% of observations) as following:

```r
pred <- predict(modelFit, testing)
```

```
## Error: object 'modelFit' not found
```

We can then assess the accuracy of our model as following:


```r
confusionMatrix(pred, testing$classe)
```

```
## Error: object 'pred' not found
```

