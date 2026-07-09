# Car Price Prediction (R Markdown)

> This document is a starter R Markdown file for the **Car Price Prediction** project.

## 1. Setup

```{r setup, message=FALSE, warning=FALSE}
# Install packages if needed
# install.packages(c('tidyverse', 'ggplot2', 'caret', 'xgboost', 'pROC'))

library(tidyverse)
library(ggplot2)
```

## 2. Load Dataset

```{r data}
# Update the path if your CSV has a different name
cars <- read.csv("car data .csv", stringsAsFactors = FALSE)

glimpse(cars)
summary(cars)
```

## 3. Basic Exploration

```{r exploration}
# Dataset dimensions
dim(cars)

# Missing values
sapply(cars, function(x) sum(is.na(x)))
```

### 3.1 Target Distribution

```{r target-dist}
ggplot(cars, aes(x = Selling_Price)) +
  geom_histogram(bins = 30, fill = "steelblue", color = "white") +
  theme_minimal() +
  labs(title = "Selling Price Distribution", x = "Selling Price", y = "Count")
```

## 4. Preprocessing (Example)

```{r preprocess}
# Feature engineering similar to the Python notebook/script
current_year <- as.integer(format(Sys.Date(), "%Y"))

# Create Age and remove Year
cars$Age <- current_year - cars$Year
cars <- cars %>% select(-Year)

# Encoding categorical variables (adjust mappings if needed)
cars$Fuel_Type <- recode(cars$Fuel_Type,
                          "Petrol" = 0,
                          "Diesel" = 1,
                          "CNG" = 2)

cars$Seller_Type <- recode(cars$Seller_Type,
                            "Dealer" = 0,
                            "Individual" = 1)

cars$Transmission <- recode(cars$Transmission,
                             "Manual" = 0,
                             "Automatic" = 1)

# Remove Car_Name (not used as numeric feature)
X <- cars %>% select(-Car_Name, -Selling_Price)
y <- cars$Selling_Price

str(X)
summary(y)
```

## 5. Model Training (Optional Placeholder)

This section uses **xgboost** in R. Tune hyperparameters as needed.

```{r model}
# Uncomment/install xgboost if required
# install.packages('xgboost')
# library(xgboost)

# NOTE: This is a template. For best results, use a proper train/test split.
# model training code example:

# set.seed(42)
# idx <- sample(seq_len(nrow(X)), size = 0.8*nrow(X))
# dtrain <- xgb.DMatrix(data = as.matrix(X[idx, ]), label = y[idx])
# dtest  <- xgb.DMatrix(data = as.matrix(X[-idx, ]), label = y[-idx])
#
# params <- list(
#   objective = "reg:squarederror",
#   eval_metric = "rmse"
# )
#
# fit <- xgb.train(
#   params = params,
#   data = dtrain,
#   nrounds = 300,
#   watchlist = list(test = dtest),
#   verbose = 0
# )
#
# preds <- predict(fit, as.matrix(X[-idx, ]))
# rmse <- sqrt(mean((preds - y[-idx])^2))
# rmse
```

## 6. Inference Template

```{r predict}
# Example single prediction row
# new_data <- data.frame(
#   Present_Price = 5.59,
#   Kms_Driven = 27000,
#   Fuel_Type = 0,
#   Seller_Type = 0,
#   Transmission = 0,
#   Owner = 2,
#   Age = 12
# )
#
# # preds <- predict(fit, as.matrix(new_data))
# # preds
```

## 7. Notes

- Your repo currently includes a Python + Streamlit flow (loading `xgb_model.json`).
- This R Markdown file is a **starter**; implement the model training/prediction steps as desired.
# CarPricePrediction
