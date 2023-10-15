# products-recommender-system-MarkovChains
Ecommerce - Modeling a Sequential Recommendation System (SRS) of Products by Markov Chains

## **Table of Contents**:

1. Objective 
2. Origin of Markov Chains
    - Modeling a Sequential Recommendation System (SRS) of Products as a DTMC
3. Data Retrieval from Relational Database
4. Data Structuring
    - Data structuring for Unsupervised Learning (ie. KMeans or PCA)
    - Data structuring for Apriori Model application
    - Data structuring for Markov Chain Model application
4. Counting of Product Pairs for Markov Chain Model
5. Transition Matrix (Frequencies & Probabilities)
6. Making Predictions with Markov Chains Transition Matrix 
6. Conclusion
    - Limitations and Recommendations for future research directions
7. Further Discussion 
    - Modeling a Sequential Recommendation System (SRS) of Products with DTMC as Long Term Analysis with Steady State Transition Matrix
8. References

### **Datasets:**

Initial dataset files with total file size [14.7 GB] from [Kaggle's eCommerce behavior data from multi category store](https://www.kaggle.com/datasets/mkechinov/ecommerce-behavior-data-from-multi-category-store)
:
- `"2019-Oct.csv"` [5.7GB]
- `"2019-Nov.csv"` [9.7GB]

`Property & Description`

- `event_time` = Time when event happened at (in UTC).
- `event_type` = 'purchase' - a user purchased a product (other 'view' & 'cart' are filtered out for this project)
- `product_id` = ID of a product
- `category_id` = Product's category ID
- `category_code` = Product's category taxonomy (code name)
- `brand` = Downcased string of brand name. Can be missed.
- `price` = Float price of a product. Present.
- `user_id` = Permanent user ID.
- `user_session` = Temporary user's session ID. Same for each user's session. Is changed every time user come back to online store from a long pause.

### **Data Normalization:**
1. Perform Data Cleaning, Data Enrichment, and Data Aggregation on an ecommerce large dataset. 
2. Reduce datasets combined file size by utilize Database Model based on Relational Database (in `.sqlite`) by Database Normalization between 3 tables with SQLite and perform queries on the Database with INNER JOIN between tables.

- **`Data Model`** built with **`PRIMARY KEY / FOREIGN KEY (fk)`** relationships​

![](kz_Database_Schema.png)

And each TABLE are linked up with **`'PRIMARY KEY/FOREIGN KEY'`** in Relational Database, created under one final `.sqlite` format. 

Accessible for stakeholders or collaborators with (i.e data engineer/data scientist/business analyst), to make SQL Query to access to the database, to derive useful information with Data Mining, or conduct Advanced Statistical Modeling and Machine Learning.

With **`-73%`** file size reduction from initial [14.7 GB]
- Database file (.sqlite): `db_connection_kz_ecommerce_2019-Oct-Nov.sqlite `- [3.9 GB]

## **Origin of Markov Chains**

*Markov chains were first introduced in 1906 by Andrey Markov, with the goal of showing that the Law of Large Numbers does not necessarily require the random variables to be independent.*

To see where the Markov model comes from, consider first an i.i.d. sequence of random variables $X_{0}, X_{1}, X_{2}, . . . , X_{n}$, . . . where we think of n as time. Independence is a very strong assumption: it means that the Xj’s provide no information about each other. At the other extreme, allowing general interactions between the $X_{j}
’s$ makes it very difficult to compute even basic things. Markov chains are a happy medium between complete independence and complete dependence.


**Stochastic Processes**

Stochastic process – a collection of random variables to represent the evolution of some system of random values over time. 

Define as $\{ {X_{n}, n \ge 0} \}$ or $\{ X_{0}, X_{1}, X_{2}, ... \}$ which can include a finite or infinite number of observations.

The space on which a Markov process "lives" can be either discrete or continuous, and time can be either discrete or continuous.

**Discrete Time Markov Chains (`DTMCs`)**

DTMC – is a special stochastic process satisfying "Markov/memoryless" (given previous state, the future is independent of the past) & "time-invariant", meaning the behavior (its response to inputs) does not change with time.

**Markov Property:**

Mathematically, for a stochastic process $\{ {X_{n}, n \ge 0} \}$ with state space S for any ${i, j, i_{0}, i_{1}, i_{2}, ··· \in S}$, we have 

$$Pr\{X_{n+1} = j|X_{n} = i, X_{n-1} = i_{0}, X_{n-2} = i_{1}, ···\}$$

1. **Markov property/Memoryless**
= $$Pr\{X_{n+1} = j|X_{n} = i\}$$

2. **Time invariant**
= $$Pr\{X_{m+1} = j|X_{m} = i\}$$ 
 
3. **Transition Probability**= 
$$P_{ij}$$


Define $P = [P_{ij}]$ as the transition probability matrix

*This says that given the history $X_{0}, X_{1}, X_{2}, . . . , X_{n}$, only the most recent term, $X_{n}$, matters for predicting $X_{n+1}$. If we think of time n as the present, times before n as the past, and times after n as the future, the Markov property says that given the present, the past and future are conditionally independent.*

*The Markov assumption greatly simplifies computations of conditional probability: instead of having to condition on the entire past, we only need to condition on the most recent value.*

reference: [Markov Chains](https://projects.iq.harvard.edu/files/stat110/files/markov_chains_handout.pdf)


### **Modeling a Sequential Recommendation System (SRS) of Products as a `DTMC`**

In an e-commerce product recommendation system, a Markov chain can be used to model the sequence of products purchased by a customer. 

The states in the Markov chain represent the products, and the transition probabilities represent the likelihood of a customer moving from one product to another. 

Hence, the product a user choose to purchase on each 'user_session' can be modeled as a Markov Chains with Transition Matrix

**DTMC as Transient Analysis with Transition Matrix**

Question that could be answered by Markov Chains model with DTMC applied as Transient Analysis with Transition Matrix for all 'product_id' product purchase sequence for all 'user_session' as states transition:

- What are the produdct a user most likely to puchase next after a purchased a current product?
