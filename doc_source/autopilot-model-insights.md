# Autopilot Model Insights<a name="autopilot-model-insights"></a>

The SageMaker model monitor report provides insights and quality information for the model candidates generated in the leaderboard of an Autopilot experiment by an AutoML job\. The report provides the model insights charts only for the best classification model candidate\. This includes understanding false positives/false negatives, tradeoffs between true positives and false positives, and tradeoffs between precision and recall\. 

Autopilot also provides scalar metrics for all of your candidate models used to measure their predictive quality\. The leaderboard view includes these metrics by default\. The metrics automatically calculated for a candidate model are determined by the type of problem being addressed\.
+ regression: MSE
+ binary classification: Accuracy, F1, AUC
+ multiclass classification: Accuracy, F1macro

You can sort your model candidates with the relevant metric to help you select and deploy the model that best addresses your business needs\. For definitions of these metrics, see the [Autopilot candidate metrics](https://docs.aws.amazon.com/sagemaker/latest/dg/autopilot-model-support-validation.html#autopilot-metrics) topic\. Her

The SageMaker model monitor report contains details characterizing the Autopilot job, a metrics table, and then several model insight charts relevant to the type of classification problem\. You access these reports in SageMaker Studio from the **Performance** tab on the page that opens to confirm that your AutoML job has completed\. For instructions on how to create and run an AutoML job in SageMaker Studio, see [Create an Amazon SageMaker Autopilot experiment](autopilot-automate-model-development-create-experiment.md)\. 

**Topics**
+ [Model details and metrics tables](#autopilot-model-insights-details-and-metrics-table)
+ [Confusion matrix](#autopilot-model-insights-confusion-matrix)
+ [The Area under the receiver operating characteristic curve](#autopilot-model-insights-auc-roc)
+ [Precision\-recall curve](#autopilot-model-insights-precision-recall-curve)

## Model details and metrics tables<a name="autopilot-model-insights-details-and-metrics-table"></a>

Model details includes the following information\.
+ Autopilot Candidate Name
+ Autopilot Job Name
+ Problem Type
+ Objective Metric
+ Optimization Direction

The Model quality information is generated by the prebuilt SageMaker Model Monitor container\. The contents of the report generated depends on the problem type addressed: regression, binary classification, or multiclass classification\. The report specifies the number of rows that were included in the evaluation dataset and the time at which the evaluation occurred\.

Here is an example of a metrics table in a model monitor report generated by AutoML job for a regression problem\.

![\[Amazon SageMaker Autopilot model insights regression metrics report example.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/autopilot/autopilot-model-insights-regression-metrics.png)

Here is an example of a metrics table in a model monitor report generated by AutoML job for a binary classification problem\.

![\[Amazon SageMaker Autopilot model insights binary classification metrics report example.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/autopilot/autopilot-model-insights-binary-metrics-report.png)

Here is an example of a metrics table in a model monitor report generated by AutoML job for a multiclass classification problem\.

![\[Amazon SageMaker Autopilot model insights multiclass metrics report example.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/autopilot/autopilot-model-insights-multiclass-metrics-report.png)

## Confusion matrix<a name="autopilot-model-insights-confusion-matrix"></a>

The confusion matrix provides a way to visualize the accuracy of the predictions made by binary and multiclass classification models for different classes\. The confusion matrix is a table that contains the percentages of correct and incorrect predictions for the actual labels\. Each row in the confusion matrix indicates how an actual label was classified by the label predicted by the model\. The percentage of accurate predictions are on the diagonal, from the upper\-left to the lower\-right corner\. The off\-diagonal percentages indicate the types of misclassification that the model is predicting\. These incorrect predictions are the confusion values\. 

Here is an example of a confusion matrix for a binary classification problem\.

![\[Amazon SageMaker Autopilot binary confusion matrix example.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/autopilot/autopilot-model-insights-confusion-matrix-binary.png)

Here is an example of a confusion matrix for a multiclass classification problem\.

![\[Amazon SageMaker Autopilot multimodel confusion matrix example.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/autopilot/autopilot-model-insights-confusion-matrix-multiclass.png)

This report provides a confusion matrix that can accommodate a maximum of 15 labels for multiclass classification problem types\. The labels are listed in order, from those predicted least accurately to those predicted most accurately\. If a row shows `Nan`, it means the validation dataset doesn't have a row for that label\.

## The Area under the receiver operating characteristic curve<a name="autopilot-model-insights-auc-roc"></a>

The area under the receiver operating characteristic curve \(AUC ROC curve\) represents the trade off between true positive and false positive rates, The AUC ROC curve is an industry\-standard accuracy metric used for binary classification models\. AUC measures the ability of the model to predict a higher score for positive examples, as compared to negative examples\. The AUC metric provides an aggregated measure of the model performance across all possible classification thresholds\. It is not dependent on the choice of a specific threshold value used to map the probabilities predicted by a model into positive and negative classifications\.

The AUC metric returns a decimal value from zero \(0\) to one \(1\)\. AUC values near 1 indicate an ML model that is highly accurate\. Values near 0\.5 indicate an ML model that is no better than guessing at random\. Values near 0 are unusual to see, and these typically indicate a problem with the data\. Essentially, an AUC near 0 says that the ML model has learned the correct patterns, but is using them to make predictions that are as inaccurate as possible \(0s are predicted as 1s and vice versa\)\. For more information about the AUC metric, see the [Receiver operating characteristic](https://en.wikipedia.org/wiki/Receiver_operating_characteristic) article on Wikipedia\.

A binary model that classifies no better than random guessing, with equal rates of true and false positives, has a AUC score of 0\.5\. The curve representing a random binary classifier is a diagonal dotted red line in a receiver operating characteristic graph\. The curves of more accurate classification models lie above this random baseline, where the rate of true positives exceeds the rate of false positives\.

![\[Amazon SageMaker Autopilot receiver operating characteristic curve example.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/autopilot/autopilot-model-insights-receiver-operating-characteristic-curve.png)

The **false positive rate **\(FPR\) measures the false alarm rate or the fraction of actual negatives that were falsely predicted as positives\. The range is 0 to 1\. A smaller value indicates better predictive accuracy\. 
+ FPR = FP/\(FP\+TN\)

The **true positive rate **\(TPR\) measures the fraction actual positives that were predicted as positives\. The range is 0 to 1\. A larger value \(1 being the largest\) indicates better predictive accuracy\.
+ TPR = TP/\(TP\+FN\)

Where these rates are defined as follows\.
+ Correct predictions
  + **True positive** \(TP\): The predicted the value is 1, and the true value is 1\.
  + **True negative** \(TN\): The predicted the value is 0, and the true value is 0\.
+ Erroneous predictions
  + **False positive** \(FP\): The predicted the value is 1, but the true value is 0\.
  + **False negative** \(FN\): The predicted the value is 0, but the true value is 1\.

## Precision\-recall curve<a name="autopilot-model-insights-precision-recall-curve"></a>

The precision\-recall curve represents the tradeoff between precision and recall for different thresholds used in a binary classification problem\. The objective of a binary classification problem is to correctly classify as many of the relevant elements that are labeled positive in a training dataset as possible\. A system with high recall but low precision returns lots of relevant results, but a high percentage of its predicted labels are incorrect when compared to the training labels\. A system with high precision but low recall returns fewer relevant results, but a high percentage of its predicted labels are correct when compared to the training labels\. A perfect system that has both high precision and high recall returns many results labeled correctly\. For more information, see [Precision and recall](https://en.wikipedia.org/wiki/Precision_and_recall) article in Wikipedia\.

**Precision** measures the fraction of actual positives that are predicted as positive out of all those predicted as positive\. The range is zero \(0\) to one \(1\)\. A larger value indicates better accuracy in the values predicted\. 
+ Precision = TP/\(TP\+FP\)

**Recall** measures the fraction of actual positives that are predicted as positive out of all of the actual positives in the sample\. This is also known as the sensitivity and as the true positive rate\. The range is zero \(0\) to one \(1\)\. A larger value indicates better detection of positive values from the sample\.
+ Recall = TP/\(TP\+FN\)

Amazon SageMaker Autopilot reports the area under the precision\-recall curve \(AUPRC\)\. The AUPRC metric provides an aggregated measure of the model performance across all possible classification thresholds\.

Here is an example that compares the precision\-recall curves and their AUPRC values from four different models trained on the same dataset\.

![\[Amazon SageMaker Autopilot precision-recall curve example.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/autopilot/autopilot-model-insights-binary-precision-recall.png)