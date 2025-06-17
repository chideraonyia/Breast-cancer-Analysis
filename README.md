# Breast-cancer-Analysis
A model that predicts cancerous status using KNN algorithm in a classification learning task

# Data Exploration
* wbcd <- wbcd[-1]
* table(wbcd$diagnosis)
* wbcd$diagnosis <- factor(wbcd$diagnosis, levels = c("B", "M"),
                         labels = c("Benign", "Malignant"))
* round(prop.table(table(wbcd$diagnosis)) * 100, digits = 1)
* summary(wbcd[c("radius_mean", "area_mean", "smoothness_mean")])
* head(wbcd)

# Data normalization i.e To make the data evenly distributed
* normalize <- function(x) {
  return ((x - min(x)) / (max(x) - min(x)))
}   
* normalize(c(1, 2, 3, 4, 5))
* normalize(c(10, 20, 30, 40, 50))
* wbcd_n <- as.data.frame(lapply(wbcd[2:31], normalize))
* summary(wbcd_n$area_mean)

# Data Preparation (Creating Training and Test Datasets)
* wbcd_train <- wbcd_n[1:469, ] 
* wbcd_test <- wbcd_n[470:569, ]
  
--{create labels for training and test data}
* wbcd_train_labels <- wbcd[1:469, 1]
* wbcd_test_labels <- wbcd[470:569, 1]
* table(wbcd_test_labels)

# Training a Model in the Data 
* library(class)
* wbcd_test_pred <- knn(train = wbcd_train, test = wbcd_test,
                      cl = wbcd_train_labels, k = 21)
# Evaluating Model Performance 
* library(gmodels)
* CrossTable(x = wbcd_test_labels, y = wbcd_test_pred,
           prop.chisq = FALSE)
# Improving Model Performance (Transforming data with z-score standardization)
* wbcd_z <- as.data.frame(scale(wbcd[-1]))
* summary(wbcd_z$area_mean)
* wbcd_train <- wbcd_z[1:469, ]
* wbcd_test <- wbcd_z[470:569, ]
* wbcd_test_pred <- knn(train = wbcd_train, test = wbcd_test,
                      cl = wbcd_train_labels, k = 21)
--{Create the cross evaluation of predicted vs actual}
* CrossTable(x = wbcd_test_labels, y = wbcd_test_pred,
           prop.chisq = FALSE)

# Testing alternative values of k
* wbcd_test_pred <- knn(train = wbcd_train, test = wbcd_test, cl = wbcd_train_labels, k=1)
CrossTable(x = wbcd_test_labels, y = wbcd_test_pred, prop.chisq=FALSE)

* wbcd_test_pred <- knn(train = wbcd_train, test = wbcd_test, cl = wbcd_train_labels, k=5)
CrossTable(x = wbcd_test_labels, y = wbcd_test_pred, prop.chisq=FALSE)

* wbcd_test_pred <- knn(train = wbcd_train, test = wbcd_test, cl = wbcd_train_labels, k=11)
CrossTable(x = wbcd_test_labels, y = wbcd_test_pred, prop.chisq=FALSE)

* wbcd_test_pred <- knn(train = wbcd_train, test = wbcd_test, cl = wbcd_train_labels, k=15)
CrossTable(x = wbcd_test_labels, y = wbcd_test_pred, prop.chisq=FALSE)

* wbcd_test_pred <- knn(train = wbcd_train, test = wbcd_test, cl = wbcd_train_labels, k=21)
CrossTable(x = wbcd_test_labels, y = wbcd_test_pred, prop.chisq=FALSE)

* wbcd_test_pred <- knn(train = wbcd_train, test = wbcd_test, cl = wbcd_train_labels, k=27)
CrossTable(x = wbcd_test_labels, y = wbcd_test_pred, prop.chisq=FALSE)
