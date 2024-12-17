# Article Recommender: Dataset Analysis and Model Results
# Business Understanding
In large organizations, users (customers, employees, or partners) interact with a variety of articles, such as support content, release notes, troubleshooting guides, and known errors. However, manually finding relevant articles tailored to a user’s profile, needs, or past behavior is inefficient. A recommender system is needed to personalize content delivery to improve user experience and efficiency.

# Business Goals
The primary business goals of the article recommender system are:
1.	Enhance User Experience:

  	- Deliver articles that are most relevant to the user based on their profile, job function, or past interactions.

  	- Reduce the time spent searching for information.

2.	Increase Content Engagement:

  	- Improve click-through rates (CTR) on articles.

    - Increase interaction rates with valuable content.
      
3.	Drive Operational Efficiency:

  	- Proactively provide solutions to known issues (e.g., support articles) to reduce helpdesk queries.

    - Help users self-solve problems, improving organizational productivity.
      
4.	Improve Customer Retention and Satisfaction:
   
    - By addressing customer pain points effectively, satisfaction improves, increasing customer loyalty.

# Dataset Overview
The data set used for analysis is 30days user behavior data. The data set contains account related data along with user data and user interaction data. For user profiling and to recommend articles, I have used 6 attributes per feedback.

# Attribute Information:
There are 3 csv files : user profile data, article item data, and interaction data. 

1. User Profile Data
   
    - APPUSERID: Unique identifier for each user.

    - AcctNewSegment: Account segmentation category.

    - AccountIndustryCategory: Industry classification.

    - AcctSegment: Account segmentation.

    - Title: User's title/role.

    - DV_U_JOB_FUNCTION: Job function of the user.

    - DV_U_JOB_LEVEL: Job level of the user.

    - DV_U_JOB_ROLE: Detailed role of the user.

2. Article Item Data
   
    - ARTICLE_NUMBER: Unique identifier for each article.

    - ARTICLE_TITLE: Title of the article.

    - CONTENT_TYPE: Type of content (e.g., "Support and Troubleshooting").

    - COMPANY: Company associated with the article.

3. Interaction Data

    - APPUSERID: User interacting with the article.

    - ARTICLE_NUMBER: Article being interacted with.

    - Target: Indicates interaction (1 = interacted, 0 = not interacted).

# Key Challenges

1.	Cold Start Problem:

    - New users or articles with no prior interactions may not receive recommendations.

2.	Data Sparsity:

    - Most users interact with only a small subset of articles, leading to a sparse interaction matrix.

3.	Content Overload:

    - A large number of articles may overwhelm users without proper filtering.

4.	Personalization:

    - Balancing general popular articles and user-specific recommendations.
  
# Evaluation Metrics

The effectiveness of the recommender system can be measured using:

1.	Precision@K: Proportion of relevant articles in the top K recommendations.

2.	Recall@K: Proportion of relevant articles successfully recommended.

3.	Coverage: Percentage of articles recommended across the entire catalog.

4.	Click-Through Rate (CTR): Percentage of recommended articles that users interact with.

# Recommender System Approach

# Analysis

# Exploratory Data Analysis (EDA)

1.	Categorical Variables:

    - Variables such as AcctSegment, CONTENT_TYPE, and DV_U_JOB_FUNCTION were visualized to understand their distributions.

    - High cardinality in columns like Title was addressed by focusing on the top 10 categories.

2.	Correlation Analysis:

     - Features like CONTENT_TYPE_Support and Troubleshooting showed high positive correlations (0.7956) with the Target variable.

    - Features such as COMPANY and APPUSERID showed minimal correlation, indicating low predictive power.

3.	Feature Engineering:

    - Encoded categorical variables using a mix of Label Encoding (for IDs) and One-Hot Encoding (for content types).

    - Generated negative samples to balance the dataset for classification tasks.

# Corelation Matrix

Key Observations

Target Correlation: 1.0: 

    - The correlation of the Target variable with itself is always 1.0, which is expected.

CONTENT_TYPE_Support and Troubleshooting: 0.7956: 

    - This high correlation suggests that the CONTENT_TYPE_Support and Troubleshooting feature has a strong positive relationship with the target variable.

    - Articles labeled with this content type are more likely to be associated with the target outcome.

CONTENT_TYPE_Known Error: 0.3088

    - A moderate positive correlation, indicating some predictive power of this feature.

CONTENT_TYPE_Release Notes: 0.1046 and others

    - These show weak correlations with the target variable, meaning their impact on predicting the target is limited.

APPUSERID: -3.479e-15

    - A correlation close to 0 indicates that APPUSERID has no meaningful relationship with the target variable. This is expected, as user IDs are unique identifiers and not predictive.

COMPANY_* Features (e.g., Paul Hastings LLP): 0.0075

    - These very low correlations indicate that the specific company associated with an interaction has minimal impact on predicting the target variable.

# Modeling and Results

Models Evaluated

1.	Support Vector Machine (SVM)

2.	Decision Tree Classifier

3.	Naive Bayes

# Results Summary

SVM:

    - Accuracy	66.66%

    - Precision	0.00%

    - Recall	0.00%

    - ROC AUC	88.10%

Metric	SVM	Decision Tree	Naïve Bayes	Random Forest	Logistic Regression
Accuracy	66.66%	99.43%	96.52%	99.44%	97.97%
Precision	0.00%	99.53%	100.00%	100.00%	100.00%
Recall	0.00%	98.77%	89.55%	98.34%	93.92%
ROC AUC	88.10%	99.27%	99.80%	99.99%	99.68%

# Interpretation

1.	SVM:
    - Poor performance due to lack of precision and recall.

  	- Likely requires better hyperparameter tuning and feature scaling.

2.	Decision Tree:

  	- Best overall performance with high accuracy (99.43%), precision (99.53%), and recall (98.77%).

    - Excellent balance between precision and recall, making it reliable.

3.	Naive Bayes:

    - High precision (100%) ensures no false positives.

    - Slightly lower recall (89.55%) compared to Decision Tree but still effective.

4. Random Forest:

   -Random Forest has a slight edge in recall and ROC AUC, making it better at identifying true positives.

   - Random Forest is slightly more balanced across metrics and excels at identifying important features.

5. Logistic Regression:
  
   - Logistic Regression has perfect precision, useful when avoiding false positives is critical.
   
   - Logistic Regression is interpretable and performs well, but it sacrifices some recall for precision.

# Recommendations

1.	Preferred Model:

    - Use Decision Tree for deployment due to its balanced performance across all metrics.

2. Further Refinement:

    - Optimize hyperparameters for the Decision Tree using GridSearch or RandomizedSearch.
  
    - Implement cross-validation to ensure robustness across different splits.

3.	SVM Tuning:

    - Apply feature scaling (e.g., StandardScaler).

    - Optimize parameters like kernel type, , and gamma to improve performance.

4.	Naive Bayes Use Case:

    - Consider Naive Bayes when avoiding false positives is critical (e.g., fraud detection).

# Next Steps

1.	Feature Importance Analysis:

    - Use feature importance from Decision Tree to prioritize key predictors.

2.	Data Augmentation:

    - Enrich the dataset with additional features, such as user activity frequency or article recency.

3.	Deployment:

    - Implement the Decision Tree model in a production environment.

    - Set up monitoring to evaluate real-world performance.

4.	Explore Hybrid Models:

    - Combine collaborative filtering and content-based filtering for enhanced recommendations.

5.	Hyperparameter Tuning:

    - Perform grid search or Bayesian optimization to fine-tune model performance.

# Business Impact

Implementing the article recommender system will have the following measurable impacts:

1.	Faster Problem Resolution:

  	- Users can access the right articles quickly, improving productivity.

3.	Higher Engagement:

  	- More personalized recommendations lead to increased interaction rates.

4.	Reduced Support Costs:

  	- Fewer helpdesk calls due to proactive article recommendations.

5.	Improved Customer Satisfaction:

    - Delivering the right content enhances user trust and satisfaction.

# Next Steps - Action Items

1.	Implement and Evaluate Models:

    - Use a Decision Tree, Naive Bayes, or other models to predict relevant articles.

2.	Deploy the Recommender System:

    - Integrate the model into a production environment.

3.	Monitor Performance:

    - Track Precision@K, Recall@K, and user engagement.

4.	Refine the System:

    - Continuously improve the model using feedback and interaction data.










