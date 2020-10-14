# ModelPerformance
Basic performance metrics of a classifier

## Requirements
Any version of [R](https://www.r-project.org/)

## Functionality
R functions are provided to plot the [receiver operating characteristic](https://en.wikipedia.org/wiki/Receiver_operating_characteristic) (ROC) or the [precision-recall curve](https://en.wikipedia.org/wiki/Precision_and_recall) (PRC) given the true outcome of a vector of observations (TRUE or FALSE) and the associate value of a predictor, such as a classifier.

## Content
### getAUC()
Compute true positive rate, false positive rate and precison as well as AUROC (area under the revceiver operator characteristics curve) and AUPRC (area under the precision recall curve).

**Parameters**:
- *real*: Ground-truth state of observations (TRUE or FALSE)
- *pred*: Value of predictor, e.g. classifier (real number in [0,1]).
- *returnRates*: Return the tableof TRP, FPR and precision
- *seed*: When multiple observations have the same pred value, they are shuffled randomly. A seed can be provided to produce reproducible results.

**Value**:

AUROC if `returnRates=FALSE`, and a list with AUROC, AUPRC and a table of predictor increments, TPR, FPR and precision if `returnRates=TRUE`.

### plotROC()
Plot receiver operating characteristic and optionally find the cutoff with largest spread and satisfying maxFPR and minPrec.

**Parameters**:
- *auc*: List containing a slot `rates` with columns `pred` (the value of a model such as a classifier), `TPR`, `FPR` and `prec`. Can be produced with `getAUC()`.
- *main*: Plot title
- *addInfo* (logical): Whether or not to attempt to find an optimal cutoff, i.e. one that maximizes the difference of TPR-FPR, that also satisfies `maxFPR` and `minPrec`.
- *maxFPR*: The maximum FPR for an optimal cutoff.

**Value**:

This function is invoked mainly for generating a ROC plot. If `addInfo=TRUE`, returns the cutoff with associated metrics.

### plotPRC()
Plot precision-recall curve and optionally cutoff with highest F1 score.

**Parameters**:
- *auc*: List containing a slot `rates` with columns `pred` (the value of a model such as a classifier), `TPR`, `FPR` and `prec`. Can be produced with `getAUC()`.
- *main*: Plot title
- *addInfo* (logical): Whether or not to attempt to find an optimal cutoff, i.e. one that maximizes the [F1 score](https://en.wikipedia.org/wiki/F1_score). 

**Value**:

This function is invoked mainly for generating a precision-recall plot. If `addInfo=TRUE`, returns the cutoff with associated metrics.


## Usage
Example input is provided:
``` R
dat <- read.delim("ExampleData.tab")
source("ROC.PRC.R")

rates <- getAUC(dat$status, dat$viralReads, returnRates=T, seed=123)
par(mfrow=c(1,2))
plotROC(rates, main="ROC", addInfo=T)
plotPRC(rates, main="PRC", addInfo=T)
```
