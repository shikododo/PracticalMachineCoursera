Practical Machine Learning - assignment writeup
========================================================

We will start by loading the caret library, and then reading all data:


```r
library(caret)
```

```
## Loading required package: lattice
## Loading required package: ggplot2
```

```r
set.seed(33833)
data <- read.csv("pml-training.csv")
```

Some variables should be excluded because most of them are NA:

```r
remove <- apply(data,2,function(x) sum(is.na(x)))
data <- data[,names(remove[remove < 1000])]
```
The total number of remaining variables is 93.
We will then divide this data into training and test sets with 60% of the data in the training set:

```r
inTrain <- createDataPartition(y=data$classe,p=0.6,list=FALSE)
training <- data[inTrain,]
testing <- data[-inTrain,]
```

We will use Random Forests prediction alogorithm on the training data set:

```r
myctrl <- trainControl(allowParallel=TRUE, method="cv", number=4)
modelFit <- train(classe~.,data=training,model="rf",trControl=myctrl)
```


To test out of sample error we need to predict our model in a new data set (the remaining 40% of observations) as following:

```r
pred <- predict(modelFit, testing)
```

```
## Loading required package: randomForest
## randomForest 4.6-10
## Type rfNews() to see new features/changes/bug fixes.
```
We can then assess the accuracy of our model as following:


```r
dd <- confusionMatrix(pred,testing$classe)
dd
```

```
## Confusion Matrix and Statistics
## 
##           Reference
## Prediction    A    B    C    D    E
##          A 2231    5    0    0    0
##          B    0 1513    1    0    0
##          C    1    0 1366    4    0
##          D    0    0    1 1282    0
##          E    0    0    0    0 1442
## 
## Overall Statistics
##                                         
##                Accuracy : 0.998         
##                  95% CI : (0.997, 0.999)
##     No Information Rate : 0.284         
##     P-Value [Acc > NIR] : <2e-16        
##                                         
##                   Kappa : 0.998         
##  Mcnemar's Test P-Value : NA            
## 
## Statistics by Class:
## 
##                      Class: A Class: B Class: C Class: D Class: E
## Sensitivity             1.000    0.997    0.999    0.997    1.000
## Specificity             0.999    1.000    0.999    1.000    1.000
## Pos Pred Value          0.998    0.999    0.996    0.999    1.000
## Neg Pred Value          1.000    0.999    1.000    0.999    1.000
## Prevalence              0.284    0.193    0.174    0.164    0.184
## Detection Rate          0.284    0.193    0.174    0.163    0.184
## Detection Prevalence    0.285    0.193    0.175    0.164    0.184
## Balanced Accuracy       0.999    0.998    0.999    0.998    1.000
```

As we can see from the confusion matrix, the overall accuracy for this model is 0.9985
