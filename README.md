### Introduction

Content note: This dataset contains real Subbredit posts, and some of the posts contain language that is not safe for work, crude, or offensive.


For Project 3, the objective is to collect and analyze posts from two distinct subreddits. Subreddits are specialized forums on the popular website Reddit that cater to specific topics and interests.

The focus will be on creating a classification model to discern the origin of a subreddit post between the following two subreddits:

**Subreddit 1: [r/AmItheAsshole]('https://www.reddit.com/r/AmItheAsshole/')**

Description: Serving as a platform for moral introspection, r/AmItheAsshole provides a space for individuals to seek feedback on contentious issues. Users present both sides of a dilemma and solicit opinions to ascertain whether their actions align with societal norms.


**Subreddit 2: [r/AskLawyers]('https://www.reddit.com/r/AskLawyers/')**

Description: Offering legal guidance, r/AskLawyers encourages users to pose questions with the understanding that professional legal advice requires consultation with an attorney. Public posts and comments in this subreddit are not construed as forming an attorney-client relationship.


The selection of these two subreddits stems from their shared purpose of providing a venue for individuals to seek input on personal matters. While one focuses on ethical considerations and societal expectations, the other centers on legal principles and potential courses of action. This juxtaposition between moral inquiry and legal inquiry presents an intriguing avenue for exploration, highlighting the intersection of public opinion and formal legal frameworks.


Data Preparation
After collecting data from the Reddit API, duplicates were removed, resulting in a dataset with 2149 entries. Some posts had missing content, which was replaced with the post title as a common practice in discussion forums.

Text Processing and Analysis
Top words in each subreddit were identified using CountVectorizer. Additionally, a custom preprocessor was implemented to remove URLs, emojis, and special characters. The lemmatize-tokenize function was applied to further process the text data.
![Most Common Words in Posts (from both sources combined)](https://git.generalassemb.ly/martafuentes/project-3/blob/master/Images/bar_most_common_all.png?raw=true)

![Distribution of Post Lengths by Word Count for Each Source](https://git.generalassemb.ly/martafuentes/project-3/blob/master/Images/bar_post_len_by_word_al_aita.png?raw=true)

Model Building and Evaluation
Two classification models were developed using different techniques. The first model utilized CountVectorizer and MultinomialNB, while the second model employed TfidfVectorizer and LogisticRegression. GridSearchCV was used to optimize model parameters.

| Model                              | Baseline Accuracy(Source 1: AmItheAssole) | Baseline Accuracy (Source 0: AskLawyers) | Best Accuracy Score (CV) | Best Accuracy Score (Training) | Best Accuracy Score (Testing) | Best Parameters Found                             |
|------------------------------------|------------------------------------|------------------------------------|--------------------------|--------------------------------|-------------------------------|--------------------------------------------------|
| CountVectorizer + MultinomialNB    | 0.719944                           | 0.280056                           | 0.964564                 | 0.979847                       | 0.950704                      | max_df': 0.95, 'max_features': 5000, 'min_df': 4, 'ngram_range': (1, 1) |
| TfidVectorizer + LinearRegression | 0.719944                           | 0.280056                           | 0.934669                 | 0.964559                       | 0.947887                      | 'max_df': 0.95, 'max_features': 2000, 'min_df': 4, 'ngram_range': (1, 1) |


| Metric          | CountVectorizer + MultinomialNB | TfidVectorizer + LogisticRegression |
|-----------------|---------------------------------|-------------------------------------|
| Specificity     | 0.889                           | 0.834                               |
| Recall          | 0.975                           | 0.992                               |
| Precision       | 0.958                           | 0.939                               |
| F1 Score        | 0.966                           | 0.965                               |


Comparison and Conclusion
Both models performed well, with minor differences in specific metrics. The F1 scores were nearly identical, indicating high precision and recall. Additionally, both models achieved high accuracy scores, suggesting their effectiveness in classifying text data.
![ROC and AUC for CVectorizer + MultinomialNB Model](https://git.generalassemb.ly/martafuentes/project-3/blob/master/Images/roc_auc_cvec_lr.png?raw=true)

![
The AUC for the ROC curve was 0.99 for both models, indicating their ability to distinguish posts from the target source.

In conclusion, whether using CountVectorizer with MultinomialNB or TfidfVectorizer with LogisticRegression, the models demonstrated strong performance in predicting the source of Reddit posts.

