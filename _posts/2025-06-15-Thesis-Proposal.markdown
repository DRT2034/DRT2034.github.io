---
layout: post
title:  "The Consumer as an Intuitive Supply Chain Manager: Forecasting and modeling"
date:   2025-06-15 18:42:51 +0200
categories: School
---
Below is a topic I'm planning to write my thesis about; Written while on a trip to Philadelphia, Pennsylvania. 

### Introduction

I’m interested in the disconnect between what people purchase and what they actually consume,
particularly in the context of grocery shopping. From a consumer finance perspective, this gap
represents a source of inefficiency, both economically and behaviorally. Most people shop at least once
a week, relying on their own judgment to estimate what they’ll need for the days ahead. In doing so,
they act as intuitive supply chain managers—mentally forecasting household demand, inventory levels,
and future needs, albeit without the structure or tools that professional planners rely on.

Based on the seminal paper by Paul Meehl, David Faust and Robyn Dawes (1989) called ‘clinical versus
actuarial judgment’, it is understood that when making predictions about people’s behavior, there is a
difference between relying on clinical judgment (intuition of the person) and actuarial judgment
(formulas and statistical models). In most cases, statistical models are found to outperform humans in
predicting behavior as they are more accurate and reliable than clinical judgment, which is heavily
influenced by biases and heuristics.

I posit that this is implicitly also the case in grocery shopping, people have a lot of variability in their
interests and are heavily influenced by this in their shopping behavior. They think they have more
change in their shopping interests over time than they actually do. I therefore aim to build a predictive
model to forecast people's shopping baskets based on past purchasing data to aid people in their
judgment process.

As measuring people’s consumption is not feasible, purchasing behavior could be modelled based on
historical data to study its predictability and variability. My assumption is that purchases are more
variable than consumption, and that this excess variability leads to inefficiencies. If purchasing behavior
can be stabilized, such as through model-assisted recommendations, then the gap between what is
bought and what is ultimately consumed could be reduced. I therefore intend to use reduction in
purchasing variability as a proxy for improved alignment between consumption and purchase, thereby
modelling purchasing behavior rather than actual consumption behavior. Building models to forecast
the individual’s market basket purchases based on past data allows me to understand the degree to which
purchases are stable over time.

My main interest for this topic is from the point of view of consumer financial management. People are
heavily influenced by personal biases and impulsive decision making that lead to inefficiencies in the
form of monetary and environmental waste. Given that retailers have limited financial incentive to get
rid of these inefficiencies, I find there to be an opportunity to analyze how predictive models could
assist individuals in achieving more financial efficiency in their shopping.

I aim to develop, evaluate and compare modelling techniques to predict individual purchase patterns,
by utilizing the latest techniques available, such as the recent appearance of “The Market Basket
Transformer: A New Foundation Model for Retail” by Gabel and Ringel (2024).

Additionally, I intend to analyze the variability of people’s shopping behavior in relation to
sociodemographic characteristics. Current literature suggests that characteristics such as age, household
composition and education level might influence the stability of purchasing patterns. For instance,
research suggests that having a higher education is associated with greater financial planning ability,
which could be correlated to lower variability in purchasing patterns. Older people are similarly
expected to have more stability in their shopping habits, while people who buy for their household
instead of just themselves are expected to have more complex needs. I intend to test these hypotheses
empirically.

My interest in this topic comes from my personal experiences. Based on data collected through GDPR
of Colruyt shopping receipts, I’ve already done various descriptive analytics and observed behavioral
trends. Simultaneously, I’ve noticed for myself that I often buy products (mainly charcuterie and dairy
products) of which I’m not sure whether I’ll be consuming them (either partly or wholly) within the
expiration period. The compounding of waste that results of such leftovers that aren’t consumed, both
financially and environmentally, motivates me to do research about it.

### Statements of the research questions

These goals translate into three main research questions. Together, they reflect the core objectives of
this thesis: assessing how predictable individual shopping behavior is, investigating how this
predictability varies across consumers, and comparing different modeling strategies for forecasting
purchases.

1. How accurately can grocery store purchases be predicted using modern basket modeling techniques from recent literature (e.g., SHOPPER, Market Basket Transformer)?

    This question forms the basis of the statistical modelling in the thesis. The aim here is to evaluate the overall predictability of purchasing behavior and how well recent models perform when applied to consumer grocery data.

2. How does the predictability of individual grocery purchases (as measured by forecast error or spending variance) relate to sociodemographic factors such as household size, education, income, and employment status?

    This question explores whether behavioral regularity is associated with demographic indicators that may also be linked to financial planning ability.

3. Which modeling approaches—time series forecasting, probabilistic basket models, or deep learning—offer the best performance in capturing individual-level purchasing patterns?

    This addresses the methodological side of the thesis, by comparing predictive performance across different modeling paradigms.

A natural extension of this work would be to test whether model-based recommendations can actively reduce the gap between what consumers purchase and what they ultimately consume. However, such an experimental design would require long-term behavioral tracking and controlled interventions, which fall beyond the practical scope of a master’s thesis. For now, the questions posed above represent the most important and feasible first step toward understanding and potentially addressing the behavioral inefficiencies.

### Data

The European General Data Protection Regulation (GDPR) provides consumers with full access to the personal data held about them by companies. As a result, individuals who use a customer card at a supermarket (e.g., Colruyt) can submit a data access request to receive a full record of their purchase history. This dataset includes product-level receipt data, linked to purchase dates, quantities, and prices. The primary data source for this research will consist of GDPR data collected from Colruyt, a major Belgian supermarket chain. I intend to recruit between 30 and 50 participants who are willing to request their data and share it alongside basic sociodemographic information, including age, education, household composition, and employment status. This individual-level data will allow for the construction of personalized predictive models and the analysis of behavioral variance in grocery shopping patterns. Regression analysis will then be used to examine how predictability varies across demographic groups.

Given the logistical difficulty of obtaining GDPR data at scale, this primary dataset will be supplemented by two publicly available retail panel datasets. The Dunnhumby Complete Journey dataset, based on purchases from Kroger (a major U.S. retailer), includes over 2,500 anonymized households tracked over two years, with detailed product, pricing, and demographic data. The Instacart Market Basket dataset, while lacking prices and demographics, provides a large volume of user–basket–product data from a U.S.-based online grocery service. Both datasets will be harmonized in currency (converted to EUR) and used to benchmark and validate modeling approaches, improving the generalizability of the findings.

These datasets are available at:
- https://www.kaggle.com/datasets/frtgnn/dunnhumby-the-complete-journey
- https://www.kaggle.com/datasets/psparks/instacart-market-basket-analysis

### Proposed statistical techniques

1. Preprocessing of Open and GDPR Datasets

    **Product normalization and categorization (GDPR data):**  
    A two-stage pipeline has been constructed in order to transform and enrich the GDPR receipt data from Colruyt. These GDPR requests are delivered in a pdf format, consisting of scanned digital receipts linked to the customer card. These pdfs must therefore be converted into a structured dataset to enable the analysis. Python package ‘pdfplumber’ is used to extract product level line items of each receipt, including the article name, quantity, unit price and total cost. The output that is created then becomes a table with transactions indexed by purchase (basket), customer and date. The dataset is then cleaned and enriched. Time-based features are extracted from the date of the receipt, aggregate spending per shopping trip, week and month is computed and basket level features such as number of items and spending per visit are created. This process allows me to transform unstructured receipt records into a dataset that is ready for analysis.

    For the categorization of the GDPR Colruyt receipt data, I built an additional pipeline that will be extended when data from other individuals is added. The pipeline starts with a rule-based classification such as ‘if milk in article name, category is Dairy’; these rules assign a rule-category label to each product name. After, a machine learning classifier (logistic regression model) is trained using the article name and the output of the rule category label set, it then uses a TF-IDF vectorizer to convert product names into numeric features and the dataset is split into train and test sets. The model is then evaluated by making predictions on the test set where accuracy and classification metrics are printed. The predictions are then compared to the original rule-based labels, and cases that don’t match are stored in a ‘needs_review’ file. I then manually correct these cases in that file (in column ‘correct_category’), after which I use these corrected examples to retrain the model and improve accuracy.

    **Other preprocessing:** The open dataset might require missing value handling (e.g., imputation or flagging), outlier detection (e.g., using Isolation Forest) and time series feature extraction (e.g., days since last purchase).

2. Feature engineering

    The feature engineering is inspired by the RFM (Recency, Frequency and Monetary) framework:

    - Recency features: Days since last purchase of a good, days since last store visit (market basket purchase).
    - Frequency features: Purchase frequency per product or category.
    - Monetary (basket) features: total spend per visit, number of items, number of distinct categories.

3. Model building

    Recent modelling advancements for consumer purchasing behavior were identified in the form of two important papers for this thesis topic:

    - **SHOPPER model (Ruiz et al., 2020):** A sequential probabilistic model that aims to predict which items a consumer would buy together, in a given shopping trip. It is a structural model of consumer behavior, and includes important aspects of the behavior such as preferences, maximized utility and information. The idea is rooted in discrete choice theory and Bayesian inference, where the basket information is treated as a sequence of product choices where decisions are based on prior selections, latent factors in the consumer’s preference (e.g., item popularity, complementary items, etc.) and context-related features such as promotions and time. This model can be used for individual level model specification with the GDPR data. This model is partly open source (GitHub repository) but seems to have been taken down. The following site rebuilds the model based on the paper, which is what I intend to replicate:  
      https://humboldtwi.github.io/blog/research/information_systems_1920/group3_shopper/

    - **Market Basket Transformer (Gabel and Ringel, 2024):** A novel transformer-based deep learning model that is designed for next-product prediction in market baskets. Transformer architecture is applied to retail data and inherent challenges of this data lying in the sparsity, scale and task diversity are addressed. The result is that the entire basket is modelled as a sequence of product “tokens”, using attention mechanisms to learn complex co-occurrence patterns between items. I anticipate that I will use this approach for benchmarking large open-source datasets such as the one from Instacart, which will allow for comparison of predictive performance across model types and contexts. This model isn’t open source as of April 2025, so I’d have to rebuild it based on open libraries. The paper provides a lot of technical details to derive the necessities from.

4. Forecast Evaluation

    The model performance will be evaluated using standard metrics:

    - Accuracy metrics: Mean Absolute Error (MAE), Root Mean Squared Error (RMSE), Mean Absolute Percentage Error (MAPE)
    - Classification metrics: F1-score, precision@k
    - Cross-entropy

5. Variance measures and regression analysis (Research Question 2)

Creating measures to showcase the behavioral variability per person:
- Spending variance: variability in weekly spending per customer
- Product variance: how often the user switches products and categories from week to week
- Forecasting error variance: the variance of the model residuals

Regression analysis: I anticipate using linear regression to model purchase variability as a function of
sociodemographic information, such as household size (or single person vs household), employment
status, age, education level etc.

Assumptions: Linearity – Normality of residuals – homoscedasticity – multicollinearity (-
endogeneity)

Consideration is also given to other (nonlinear) models in case linear regression shows a poor fit. These
assumptions will not need to be evaluated for the non-parametric models such as SHOPPER and MBT.
Literature study

The literature relevant to this topic spans two main streams: one rooted in the psychology of intuitive
judgment, and the other in predictive modeling of consumer behavior. The theoretical foundation is
provided by Meehl et al. (1989), whose seminal work contrasts clinical (intuition-based) and actuarial
(statistical) judgment. Across various domains, they demonstrate that algorithmic models consistently
outperform human intuition due to their greater reliability and resistance to bias. This supports the idea
that model-based forecasts could improve decision-making in domains such as grocery shopping, where
consumers often act on heuristics.

The modeling component of this thesis builds on two recent contributions. Ruiz et al. (2020), who
introduced the SHOPPER model: a probabilistic, sequential model of individual purchasing behavior
incorporating latent preferences and contextual factors. The second, and most recent, contribution was
that of Gabel and Ringel (2024), who developed the Market Basket Transformer, a transformer-based
deep learning model that leverages attention mechanisms to model complex co-purchase structures and
achieve state-of-the-art performance in basket prediction.

### Future reading list for the eventual literature review:

Judgment and prediction science:
- Dawes, R. M., Faust, D., & Meehl, P. E. (1989). Clinical versus actuarial judgment. Science,
243(4899), 1668-1674.
Consumer purchase behavior modelling:
- Armstrong, R. (2008). The Long Tail: Why the Future of Business Is Selling Less of More.
- Ruiz, F. J., Athey, S., & Blei, D. M. (2020). Shopper. The Annals of Applied Statistics, 14(1),
1-27.
- Gabel, S., & Ringel, D. (2024). The Market Basket Transformer: A New Foundation Model
for Retail. Available at SSRN 4335141.
- Manchanda, P., Ansari, A., & Gupta, S. (1999). The “shopping basket”: A model for
multicategory purchase incidence decisions. Marketing science, 18(2), 95-114.
Linking demographics and traits to impulsive and planned spending:
- Donnelly, G., Iyer, R., & Howell, R. T. (2012). The Big Five personality traits, material
values, and financial well-being of self-described money managers. Journal of economic
psychology, 33(6), 1129-1142.
- Antonides, G., De Groot, I. M., & Van Raaij, W. F. (2011). Mental budgeting and the
management of househ