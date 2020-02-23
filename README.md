# Stacked Adversarial Classifier

The great to and fro in cybersecurity using machine learning is the improvement of TPR and FPR rates on unsupervised algorithms. Cluster and OD algorithms like One Class SVM will have either a decent recall, or an almost zero recall but high precision.

Stacked adversarial classifier techniques aim to improve the TPR and FPR simultaneously by taking unsupervised algorithms like the ones found in PyOD and Scikit-Learn and boosting them with a supervised classifier trained on a specially curated dataset.

Supervised algorithms with labeled data on rare events often suffer from a lack of generalizability. The severe class imbalance often leads to memorization of the outlier structures that quickly breaks down on test sets and deployment. Even with resampling techniques, it's still shuffling the cards with extremely small amounts of test data.

Additionally, in real world cybersecurity scenarios, it is often far easier to construct a "normal" representative dataset and tuning is done based on analyst feedback of TPR-FPR exhaustion. There may be a few labeled data points with which to trace thresholds with. However, due to the shifting style of attack patterns, these may lose their predictive power quickly.

# Stacking as an adversarial technique

Stacking is the method constructing a data set for a supervised algorithm that purposefully includes the same data (or minor variations on the same data) with different labels. It only supports binary classification, but creates a more robust model for outliers because the replication of data in both labels counter acts memorization.

The desired class to predict is labeled 0, and the entire data set is stacked on top of it and labeled 1.

The algorithm then classifies based on whether it can discern between the desired label (which can be a dataset of just normal data, or outlier data) and the entire dataset. If the algorithm struggles to determine whether a data point is either in the 0 class or the 1 class then that data point likely belongs to the 0 class. While a high certainty of 1 means that it is distinctly different from the structure of the 0 class.

By leveraging confusion as the metric as well as reinforcing a more generalized representation of the target class, it improves on standard classification techniques.

# Efficacy

It was found in testing and developing that if a fraud dataset has a significant portion (~90%) of known good behavior, the normal dataset can be labled 0 as the prediction dataset. Thus, the classifier learns the structure of the normal data and the anomalous/fraud/threat data is distinguished as significantly different.

However, the BENCHMARK_v2 notebook was meant to simulate a more real world benchmark where labeled data is not available. Instead, the data is run through a histogram based outlier algorithm, labeled, and the outlier labels are used as the target data. After running it through a Random Forest classifier, it not only improved the TPR-FPR ratio, but also recall and did better than a standard supervised classification approach.
