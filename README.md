## Topic Modeling and Classification

The goal of this project is to build a classifier for Persian tweets. Since the dataset of 600,000 tweets was not labeled, the first step was to extract topics from the data. After experimenting with different topic modeling techniques (LDA, CTM, and BERTopic), **BERTopic with K-Means** clustering was chosen based on the results (illustrated [here](https://github.com/FatemehValeh/Topic-Modeling)).

For each value of \( K \) from 2 to 19, BERTopic was executed with K-Means clustering. A subset of the dataset was manually labeled to create initial labels. Each labeled tweet was then assigned to the closest cluster, and the label for each cluster was determined by the majority class of tweets within it. Finally, a **logistic regression classifier** was trained on this labeled data, and the performance was evaluated using precision, recall, and F1-score.
