# Topic Modeling and Classification of Persian Tweets

The goal of this project is to build a classifier for Persian tweets. Since the dataset of 600,000 tweets was not labeled, the first step was to extract topics from the data. After experimenting with different topic modeling techniques (LDA, CTM, and BERTopic), **BERTopic with K-Means** clustering was chosen based on the results (illustrated [here](https://github.com/FatemehValeh/Topic-Modeling)).

## 1. Effect of \( K \) in K-Means on Clustering and Classification  
For each value of K from 2 to 19, BERTopic was executed with K-Means clustering. A subset of the dataset was manually labeled to create initial labels. Each labeled tweet was then assigned to the closest cluster, and the label for each cluster was determined by the majority class of tweets within it. Finally, a logistic regression classifier was trained on this labeled data, and the performance was evaluated using precision, recall, and F1-score.

As shown in the plot below, increasing \( K \) initially leads to a notable increase in **average cluster purity**, especially at lower values of \( K \), as clusters become better differentiated by topic. Around \( K = 13 \), the average purity stabilizes, indicating that the model has sufficiently distinguished different topics. However, increasing \( K \) to very high values—eventually approaching the total number of tweets—results in an average purity of 1, as each tweet forms its own individual cluster.  

![Clustering Purity vs. K](effect%20of%20k%20on%20the%20average%20purity.png)  

Additionally, increasing \( K \) improves **classification performance** in terms of **precision, recall, and F1-score**, particularly at the beginning, as tweets become more distinguishable from one another. However, similar to cluster purity, performance stabilizes around \( K = 13 \), suggesting that the classifier benefits from well-formed topic clusters but does not gain further improvements beyond this point.  

![Classification Performance vs. K](effect%20of%20k%20on%20classification%20results.png)  

---
## 2. Execute BERTopic, Label Data, and Classify  

For each value of \( K \) from 2 to 19, BERTopic was executed with K-Means clustering. Tweets in each cluster were labeled using the **Name** generated by BERTopic for that cluster. A logistic regression model was then trained and tested on the labeled data.  

The plot below shows the effect of \( K \) on classification performance. Unlike the previous experiment, where clusters were labeled based on the majority class of manually labeled tweets, here the **BERTopic-generated names** were used directly as labels.  

Since cluster names consist of separated words (e.g., *"رژیم، شاه، اسرائیل"*), two clusters might contain similar tweets but have different names due to word order or minor differences in word selection. As a result, clusters with **semantically similar content but different labels** negatively impact classification performance.  

![Classification Performance using BERTopic Labels vs. K](effect%20of%20K%20on%20classification%20performance%20using%20BERTopic%20names%20as%20labels.png)  


## 3. Refining BERTopic Labels Using GPT-4 for Classification  

In this experiment, we aimed to improve the quality of cluster labels to enhance classification performance. Instead of using the raw **Names** generated by BERTopic, which often contained unordered or redundant words, we refined them using **GPT-4**.  

### **Methodology**  
1. **Generating Initial Topics:**  
   - BERTopic was executed with **K = 16**.  
   - The **Names** assigned to clusters by BERTopic (e.g., *"رژیم، شاه، اسرائیل"*) were extracted.  

2. **Refining Topics Using GPT-4:**  
   - The extracted names were given to **GPT-4**, prompting it to identify more general and meaningful topics.  
   - This step reduced redundancy and improved semantic clarity in cluster labels.  

3. **Classification:**  
   - The refined labels were used to assign labels to tweets.  
   - A **logistic regression model** and an **MLP model** were trained and tested on the newly labeled data.  

4. **Final Evaluation:**  
   - Both models were evaluated using a **manually labeled dataset** (not used in training).  
   - Performance was measured in terms of **precision, recall, and F1-score**, as shown in the table below.
  
### **Performance Comparison: Before vs. After Refining Labels**
| Labeling Approach                  | Number of Classes | Precision | Recall | F1-Score |
|------------------------------------|------------------|-----------|--------|----------|
| Raw BERTopic Cluster Names        | 16               | 0.41      | 0.40   | 0.40     |
| GPT-4 Refined Topic Labels        | 6                | 0.52      | 0.48   | 0.49     |

### **Final Classification Results**

| Model                 | Precision | Recall | F1-Score |
|----------------------|-----------|--------|----------|
| Logistic Regression | 0.52      | 0.48   | 0.49     |
| MLP                 | 0.66      | 0.65   | 0.65     |

📌 **Key Finding:** Refining BERTopic-generated labels with **GPT-4** improved classification performance by reducing label inconsistencies. The **MLP model outperformed logistic regression**, demonstrating that a more complex model benefits from the improved topic representations.  

