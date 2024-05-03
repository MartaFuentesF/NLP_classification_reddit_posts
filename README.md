## Using NLP and Modeling Techniques to Determine the Source of Posts from Two Different Subreddits

### Introduction

This project's objective is to collect and analyze posts from two distinct subreddits. Subreddits are specialized forums on the popular website Reddit that cater to specific topics and interests.

The focus will be on creating a classification model to discern the origin of a subreddit post between the following two subreddits:

Content note: This dataset contains real Subbredit posts, and some of the posts contain language that can be crude or offensive.

Subreddit 1: [r/AmItheAsshole]('https://www.reddit.com/r/AmItheAsshole/')

Description (from the forum): r/AmItheAsshole is a platform for moral introspection. It allows individuals to seek feedback on contentious issues. Users present both sides of a dilemma and solicit opinions to ascertain whether their actions align with societal norms.


Subreddit 2: [r/AskLawyers]('https://www.reddit.com/r/AskLawyers/')

Description (from the forum): Offering legal guidance, r/AskLawyers encourages users to pose questions with the understanding that professional legal advice requires consultation with an attorney. Public posts and comments in this subreddit are not construed as forming an attorney-client relationship.


The selection of these two subreddits stems from their shared purpose of providing a venue for individuals to seek input on personal matters. While one focuses on ethical considerations and societal expectations, the other centers on legal principles and potential courses of action. While one focuses on ethical considerations and societal expectations, the other centers on legal principles and possible consequences. In both cases, people want advice on a course of action.


### Data Preparation

After collecting data from the Reddit API, duplicates were removed, resulting in a dataset with 2149 entries. There are three columns: title, post, and source. The data is unbalanced, as ~72% of the posts originated from AITA. This was addressed by stratifying the target variable. Missing data from the 'post' column was imputed using the post title; it is common practice in discussion forums to use the title line for a question.

### Text Processing and Analysis

The top words in each subreddit were identified using CountVectorizer. A custom preprocessor was also implemented to remove URLs, emojis, and special characters. A lemmatize-tokenize function was applied to further process the text data.

![Most Common Words in Posts (from both sources combined)](https://git.generalassemb.ly/martafuentes/project-3/blob/master/Images/bar_most_common_all.png?raw=true)

![Distribution of Post Lengths by Word Count for Each Source](https://git.generalassemb.ly/martafuentes/project-3/blob/master/Images/bar_post_len_by_word_al_aita.png?raw=true)

![Distribution of Post Lengths by Character Count for Each Source](https://git.generalassemb.ly/martafuentes/project-3/blob/master/Images/bar_post_len_by_char_al_aita.png?raw=true)


### Model Building and Evaluation

Two classification models were developed using different techniques. The first model used CountVectorizer and MultinomialNB, while the second model employed TfidfVectorizer and LogisticRegression. GridSearchCV was used to optimize model parameters. The tables below summarize the accuracy metrics and parameters for the two different models.



| Model                              | Baseline Accuracy (Source 1: AmItheAssole) | Baseline Accuracy (Source 0: AskLawyers) | Best Accuracy Score (CV) | Best Accuracy Score (Training) | Best Accuracy Score (Testing) | Best Parameters Found                             |
|------------------------------------|------------------------------------|------------------------------------|--------------------------|--------------------------------|-------------------------------|--------------------------------------------------|
| CountVectorizer + MultinomialNB    | 0.719944                           | 0.280056                           | 0.964564                 | 0.979847                       | 0.950704                      | max_df: 0.95, max_features: 5000, min_df: 4, 'ngram_range': (1, 1) |
| TfidVectorizer + LinearRegression | 0.719944                           | 0.280056                           | 0.934669                 | 0.964559                       | 0.947887                      | max_df: 0.95, max_features: 2000, min_df: 4, 'ngram_range': (1, 1) |


### Confusion Matrix for CountVectorizer + MultinomialNB
![Confusion Matrix for CountVectorizer + MultinomialNB](https://git.generalassemb.ly/martafuentes/project-3/blob/master/Images/conf_matrix_cvec_mnnb.png?raw=true)


### Confusion Matrix for TfidVectorizer + LogisticRegression
![Confusion Matrix for TfidVectorizer + LogisticRegression](https://git.generalassemb.ly/martafuentes/project-3/blob/master/Images/conf_matrix_tvec_lr.png?raw=true)

### Logistic Regression as an Inferential Model

In addition to making predictions in a classification scenario, Logistic Regression models can give us inferential insights by using the coefficients associated with the features. These are determined by the model after searching for the best hyperparameters. In this case, positive coefficients suggest words associated with one class, while negative coefficients suggest words related to the other. Below are top ten influential  words and their coefficients for posts in AskLawyers.

| Coefficient | Word     |
|-------------|----------|
| -2.248      | court    |
| -2.042      | legal    |
| -1.862      | lawyer   |
| -1.796      | Is       |
| -1.750      | case     |
| -1.518      | Can      |
| -1.478      | property |
| -1.438      | police   |
| -1.351      | any      |
| -1.323      | What     |





### Comparison and Conclusion

In addition to accuracy, both models were evaluated using the results of a confusion matrix. Accuracy is not as informative of a metric as the ones below.

| Metric          | CountVectorizer + MultinomialNB | TfidVectorizer + LogisticRegression |
|-----------------|---------------------------------|-------------------------------------|
| Specificity     | 0.889                           | 0.834                               |
| Recall          | 0.975                           | 0.992                               |
| Precision       | 0.958                           | 0.939                               |
| F1 Score        | 0.966                           | 0.965                               |

Both models performed well, with minor differences in specific metrics. The F1 scores were nearly identical, indicating high precision and recall. Additionally, both models achieved high accuracy scores, suggesting their effectiveness in classifying text data.


![ROC and AUC for CVectorizer + MultinomialNB Model](https://git.generalassemb.ly/martafuentes/project-3/blob/master/Images/roc_auc_cvec_lr.png?raw=true)


![Roc and AUC for TfidVectorizer + LogisticRegression Model](https://git.generalassemb.ly/martafuentes/project-3/blob/master/Images/roc_auc_tvec_lr.png?raw=true)

Both models' AUC for the ROC curve was 0.99, indicating their effective ability to distinguish posts from the target source.

In summary, powerful NLP tools like CountVectorizer and TfidVectorizer, used in conjunction with Pipelines and Gridsearch CV, can be used to make effective predictive and inferential models. Whether using CountVectorizer with MultinomialNB or TfidfVectorizer with LogisticRegression, the models demonstrated strong performance in predicting the source of Reddit posts.

*citation:* Throughout my notebooks, I used Grammarly to help improve my technical writing.