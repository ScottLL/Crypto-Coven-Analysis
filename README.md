---
title: "Crypto-Coven-Analysis"
author: "Scott Lai, Zhonglin Wang, Dany Jabban"
date: "2022-12-4"
---

## Abstract  

In this analysis we are using the Crypto Coven NFT data set to try and answer two questions, what factors influence the likelihood of a Crypto Coven sale and what factors are associated with the increase in Crypto Coven price. The motivation and working hypothesis before conducting this analysis was that feature rarity was a major driver for both components. Our results indicate that this hypothesis was partially true, the rareness of a characteristic is highly correlated with price; however, the likelihood of a sale is most strongly associated with upfront and obvious characteristics like facial structure and skin tone. 

![Subject 2](https://user-images.githubusercontent.com/43226003/205502131-515b478f-f912-483e-8f15-acc02570840d.png)


## Introduction  
#### Background
Non-fungible Token is referred to as NFT. Non-fungible essentially says that anything is unique and cannot be changed with another object. The majority of NFTs are created using digital art, and this art is composed of a variety of rare components that determine the NFT's selling price. Rarer things are typically more expensive. Contrary to popular belief, users can purchase NFTs regardless of their rarity and still collect those NFTs by their interests. We are attempting to determine what elements influence the NFT price and sell status, even though a 100% accurate technical analysis for the NFTs is unlikely.

Aletheia and Nyx, the creators of Crypto Coven, enjoyed the concept of holding a nonfungible currency (NFT) in the form of digital art but couldn't find any portraits that accurately represented them as female tech workers and hobby artists. In order to stand out for their community, they created the Crypto Coven NFT.

#### Data source
All Crypto Coven NFT's, along with their features and subcategories can be found on the OpenSea digital marketplace where they can be purchased. The data set was put together on Kaggle contains information about 9761 witches from Crypto Coven NFT object collected using OpenSea API. It contains 45 different variables including several identification variables, sale price, number of sales, with the remaining being visual characteristics of the Crypto Coven witches. These characteristics include skin tone, hair style, jewelry, etc, and comprise 38 of 45 variables. The majority of these features are categorical with the number of subcategories ranging from 3 to over 40. This data contained 9764 NFT's with roughly half being sold and half not sold. 

Before conducting any analysis, we had the hypothesis that rarity is the most important factor in driving demand and price for Crypto Coven NFT's. This led us to want to answer two questions:

*"What factors determine whether an NFT artwork will be sold or not?"*

*"What factors determine the price that a certain NFT artwork will be sold at in the marketplace?"*


Both questions are inference based as we are trying to identify associations and directions of relationships. The first question is concerned with the binary outcome variable sale status and the second question deals with a continuous outcome variable price.

The NFT market is both extremely popular and noisy with millions of dollars being exchanged without much sense of purpose. Therefore our goal is really to make sense of whats going on in the market and to see if there is any signal in this chaotic environment which could potentially give us an upper hand in selecting certain NFT's.



## Methods

#### Data Cleaning  

Our initial data set was comprised of 45 variables. Once we determined our outcome variables (sale price and sale status), we removed those from the pool of predictor variables. Additionally, our data set had several identifier variables such as "name", "description", "id", "token_id", etc. which provided no additional information so this left us with 33 variables for our predictor space. This is still a large number of predictors so we will do some further preprocessing and variable consolidation/combination.

* Our NFT's have a set of variables known as "soft skills" which include Wonder, Wisdom,	Will,	Wit, Wiles, and Woe. These are all discrete continuous variables in the range $0 - 10$. We will combine these 6 variables by summing their values for each NFT which will result in a single column consisting of integers in the range $0 - 60$.

* Our NFT's also have a set of variables known as "props". These variables are sparsely populated since most most of the NFT's dont have "props" and an even smaller subset have all of them. Thus we will convert each of these "prop" variables into binary variables meaning you either have that type of "prop" or you don't. We will then combine these variables by summing their values for each NFT just as we did for the "soft skills." This results in a single predictor variables consisting of integers in the range $0 - 12$. For reference these variables are the following: Necklace, Hat, Hair (Back), Face Markings, Facewear, Hair Topper, Back Item, Earrings, Forehead Jewelry,	Hair (Middle), Mask, and Outerwear.

* The remaining predictor variables are the categorical features of the NFT. These include: Skin Tone, Rising Sign,	Eyebrows,	Body Shape,	Moon Sign, Hair Color, Sun Sign, Eye Style, Eye Color, Mouth, Archetype of Power, Hair (Front), Top, and Background. There is no further preprocessing done on these variables and will remain separate multi-categorical predictors.


#### EDA  

We used histograms to check the distributions of our discrete continuous predictors soft skills and props. We also wanted to check the distributions of our outcome variables price and sale status. Since the bulk of our predictors were categorical there was no real concern for multicolinearity. 

We also wanted to check if there were any clear relationships between characteristic frequency and price used bar plots to visualize these relationships. The most informative findings are discussed in our results. 

#### Logistic Regression Model  

To answer the first question of what features are most strongly associated with sale status we used a logistic regression model since our outcome variable is binary.

For this part of the analysis we are trying to determine what features of the coven witches are most strongly associated with the odds that the nft will be sold or not. In the previous section we conducted some feature engineering as well as some a priori variable selection to reduce and improve the interpretability of our feature space. After this was applied we still had feature space of 16 features with the vast majority of predictors being categorical with number of subcategories ranging from 10 to 40 thus interpretability is still quite difficult. In addition, the logistic regression model fit with all 16 predictors resulted in perfect separation and thus could not converge. This means our feature space is still too large. In order to remedy this we have applied step-wise variable selection with AIC criterion to select the most informative predictors. We chose the AIC - criterion for its medium strength parameter penalty since 16 is large but not an overwhelming number of predictors. This process resulted in our final model consisting of just two predictors Skin Tone and Body Shape. 


* logit model assessment

In order to assess model fit, we applied a binned residual plot on our model. 
<img width="709" alt="Screen Shot 2022-12-04 at 11 34 32 AM" src="https://user-images.githubusercontent.com/43226003/205503327-f1359f27-1a46-4ba9-a758-0a56b64b85b1.png">


#### Linear Regression Model 

To answer the second question of what features are most strongly associated with sale price we used a linear regression model since our outcome variable is continuous.

We conducted a stepwise selection of AIC criterion to select the best linear regression model because 16 variables are not too many and BIC criterion is too powerful in this case. AIC provides a medium strength parameter penalty.   

After the AIC-selection, we choose our final linear regression model to determine what features are associated with the increase in NFT's price.  

* linear model assessment

In order to assess model fit, we applied 4 residual plot on our model to check if  model assumptions are being met or not met.

![Screen Shot 2022-12-04 at 11 35 25 AM](https://user-images.githubusercontent.com/43226003/205503375-221dc0dd-5390-47e6-a128-ae1532a12e47.png)

# Results  

### EDA  

#### Plots for Soft Sklls count, Props Count, Total Price (Unit: Gwei) Count, Sold or Non-Sold 


![Screen Shot 2022-12-04 at 11 14 29 AM](https://user-images.githubusercontent.com/43226003/205502266-d2a2a266-8861-4698-92b4-b8c7e7fb8802.png)

From the Frequence plot we can see the total price count distribution across the range, we can find out most Crypto Coven NFTs are low price, only a few NFTs been sold over $5e^{18}$ Gwei (5 ETH or $6275). The sale status count bar plot indicates that we have almost the same proportion of NFT's sold as NFT's not sold. This allows for simple interpretations of our logistic regression model results since our outcome variable is balanced. We also see that soft skills and props are roughly normally distributed and so Crypto Coven NFT's with very high or very low values are rare and is useful information for our analysis.


#### Bar plots for Body shape count, eyebrows count, background count, and Skin.Ton. count.  

![Screen Shot 2022-12-04 at 11 15 26 AM](https://user-images.githubusercontent.com/43226003/205502315-04004080-f468-4a22-b223-f0e9b9eb1fe2.png)

These 4 variables Skin.Tone, Body.Shape, Eyebrows, and Background are the most important variables since they are included either in our final logistic model or in the linear model. They are all multi-categorical variables and as we can see the counts of all subcategories of skin.Tone and Body.Shape are similar, while some subcategories of Eyebrows and Background are very rare and some has high frequency. The difference between these subcategories matters when we go further to reveal the relationship between the odds of sale and counts and the relationship between the price of NFTs and the counts of these features. Therefore, our next step is to give analysis using the models and present comparisons of rarity and the outcome variable we want to analyze (odds of sale and the price).  

#### Comparison of the price and counts of subcategories of Skin.Tone, Eyebrows, Background  
![Screen Shot 2022-12-04 at 11 17 09 AM](https://user-images.githubusercontent.com/43226003/205502393-3ba518a2-db9e-4c83-bcc4-8c97c525574e.png)
For Skin.Tone, it seems that the distributions of counts and prices among diverse subcategories are not distinct. However, for eyebrows and backgrounds, some very rare subcategories have relatively high or even the highest mean value of last sale total price. For example, the Solid(Peach) and Solid(Lavender) background are very rare but they are the two subcategories with the highest and the second highest price among all types of backgrounds. Our conclusion in this part is that in terms of the price, the rarity of some features does provide high prices.  

Since this conclusion contradicts the previous conclusion we obtained from the EDA part in the odds of sale analysis in a sense, we need further conduct the logistic regression model and linear regression model to check whether our hypothesis holds.


### Final Linear Regression Model  


#### Linear Regression Model Assessment 
.

After fitting the model we want to check that the model assumptions are not violated
See Appendix for residual plots of linear model 

\textbf{1. linearity:}  

The p-value < 0.05, and therefore we reject the null hypothesis and conclude that the linear regression model is significant and there exists a linearity between the predictive variables and the price.  

To check if the model that we selected follows the linear regression assumptions, we will plot the model summary plots and also interpret the F-statistic in the model results. The resulting plots are available in Appendix C.

* Residuals and Fitted Plots: There is no pattern in the residual plot thus there is an independence of errors. 
* Scale-Location Plot: The residuals are equally distributed over the range of predictors so we have Homogeneity assumption.
* Normal Q-Q plot: All points are distributed along the reference line so residuals are normally distributed.
* Residual vs Leverage Plot: There are no points outside the boundry so no high leverage points to be concerned with.



$$\hat{Price_i} = \beta_0 + \beta_1 * Skin.Tone_i + \beta_2 * Eyebrows_i + \beta_3 * Background_i + \epsilon_i$$  
<img width="932" alt="Screen Shot 2022-12-04 at 11 18 00 AM" src="https://user-images.githubusercontent.com/43226003/205502435-5cebb50b-7d04-4052-bf5d-5dea234cde4d.png">


For this part of the price prediction, our goal of analysis is to determine what features of the coven witches are most strongly predict the last sale price. In the previous analysis we performed a priori stepwise-AIC variable selection to reduce the variables we include in the model and improve the predictive performance of our model. After this process we obtain only four variables, and they are Skin.Tone, Eyebrows, and Background. They are multi-categorical.  

Our model shows that with a significance level of 0.012 < 0.05, with other features held constant, compared to Dawn, as Skin.Tone being Night, the estimated last_sale.total_price will increase by -1.470634e+17 unit. In addition, we are 95% confident that the true value of the increase in last_sale.total_price is between -2.62e+17 and	-3.19e+16.  

Our model shows that with a significance level of 0.037 < 0.05, with other features held constant, compared to None, as Eyebrows being Arched (Brown), the estimated last_sale.total_price will increase by -2.95e+17 unit. In addition, we are 95% confident that the true value of the increase in last_sale.total_price is between -5.73e+17 and -1.72e+16.  

Our model shows that with a significance level of 0.010 < 0.05, with other features held constant, compared to None, as Eyebrows being Bushy (Brown), the estimated last_sale.total_price will increase by -3.65e+17 unit. In addition, we are 95% confident that the true value of the increase in last_sale.total_price is between -6.42e+17 and -8.70e+16.  

Our model shows that with a significance level of 0.005 < 0.05, with other features held constant, compared to None, as Eyebrows being Bushy (White), the estimated last_sale.total_price will increase by -3.966732e+17 unit. In addition, we are 95% confident that the true value of the increase in last_sale.total_price is between -6.74e+17 and -1.19e+17.  

Our model shows that with a significance level of 0.026 < 0.05, with other features held constant, compared to None, as Eyebrows being Pencil (White), the estimated last_sale.total_price will increase by -3.17e+17 unit. In addition, we are 95% confident that the true value of the increase in last_sale.total_price is between -5.95e+17 and -3.83e+16.  

Our model shows that with a significance level of 0.006 < 0.05, with other features held constant, compared to None, as Eyebrows being Round (Navy), the estimated last_sale.total_price will increase by -3.92e+17 unit. In addition, we are 95% confident that the true value of the increase in last_sale.total_price is between -6.70e+17 and -1.15e+17.  

Our model shows that with a significance level of 0.012 < 0.05, with other features held constant, compared to None, as Eyebrows being Strong (Black), the estimated last_sale.total_price will increase by -3.55e+17 unit. In addition, we are 95% confident that the true value of the increase in last_sale.total_price is between -6.32e+17 and -7.84e+16.  

Our model shows that with a significance level of 0.018 < 0.05, with other features held constant, compared to None, as Eyebrows being Thin Arched (Grey), the estimated last_sale.total_price will increase by -3.35e+17 unit. In addition, we are 95% confident that the true value of the increase in last_sale.total_price is between -6.11e+17 and -5.80e+16.  

Our model shows that with a significance level of 0.000 < 0.05, with other features held constant, compared to Lavender, as Background being Solid (Lavender), the estimated last_sale.total_price will increase by 2.24e+18 unit. In addition, we are 95% confident that the true value of the increase in last_sale.total_price is between 1.15e+18 and	3.32e+18.  

Our model shows that with a significance level of 0.000 < 0.05, with other features held constant, compared to Lavender, as Background being Solid (Peach), the estimated last_sale.total_price will increase by 6.21e+18 unit. In addition, we are 95% confident that the true value of the increase in last_sale.total_price is between 4.68e+18	and 7.75e+18.  

### Final Logistic Regression Model 

#### Logistic Regression Model Assessment

After fitting the model we want to check that the model assumptions are not violated
See Appendix for residual plots of linear model 

To assess the fit of our logistic regression model we used a binned residual plot. The binned residual plot has all the residuals within the error bounds and thus our logit model satisfies the requirements and is a valid model for our data. 
<img width="922" alt="Screen Shot 2022-12-04 at 11 31 44 AM" src="https://user-images.githubusercontent.com/43226003/205503145-66c2eb64-2e6c-419a-8006-84dce3d9cff5.png">


From our final model only 1 sub feature was statistically significant. Our model shows that with a significance level of 0.006, with body shape held constant, the odds that a Coven Witch NFT is sold increases by 24% if the skin tone is "Midnight" instead of the baseline "Dawn". In addition, we are 95% confident that the true value of the increase in sale odds for Midnight skin tone is between 6.3% and 44%.

While Body Shape: "Soft" and Skin Tone: "Day" do not have a significance level below the 0.05 standard threshold, they do have significance levels 0.075 and 0.052, respectively, which is less than the 0.1 threshold and thus I believe they could still provide some useful information. Our model shows that with a significance level of 0.052, with body shape held constant, the odds that a Coven Witch NFT is sold decreases by 13% if the skin tone is "Day" instead of the baseline "Dawn".
We are 95% confident that the true value of the decrease in sale odds for "Day" skin tone is between 25% and 0%.
Our model also shows that with a significance level of 0.075, with skin tone held constant, the odds that a Coven Witch NFT is sold increases by 9.6% if the body shape is "soft" instead of the baseline "Chiseled". We are 95% confident that the true value of the increase in sale odds for "Soft" body shape is between 0% and 22%. The exponentiated 95% confidence interval for all predictors is located in the appendix.

The proportional bar plot above shows these sale proportion differences between the subcategories visually. We can see that midnight skin tone Crypto Coven's have a larger proportion of sales compared with the other skin tones and the day skin tone has a lower proportion of sales when compared with the rest of the skin tones. We also see this proportional sale difference with body shape but albeit a much more subtle difference. The soft body shape has a slightly higher proportion of sales compared to the other body shapes. This matches the relationships our logistic regression model identified.


## Conclusion  
As the result,we can find out that Skin tone and Body Shape are the most significant variables for NFT sale. After people make the buy decision, the factor of rarity is joined to affect the NFT price change. The rarer the elements, the higher the price. For example the eyebrow White and background peach are the rarest subcategories provide the highest sell compare with others. The evidence show that most people buy Crypto Coven NFTs with their first impression of preference for the NFTs. The appearance features are associated with buying or not buying, and the properties' rarity affects the buying price.

#### Shorage 
- As we conduct the model by adding the variables as one factor for soft skills and props, we might lose some detailed affections with those subcategories. We might continue working with soft skills and props for future analysis to see if the subcategories affect the predictor variables. 

- the data only contains selling information before 2020 which is out of date. We are trying to apply the OpenSea API in the future to get updates data and informations.

#### Challenge 
- Due to the properties of NFT diversities, the model result might only fit some NFT projects across the board. As an example of a computer generating NFTs, Crypto Coven can represent this specific form and its communities. The model result might only reflect the buying preference in these communities. We need to do some more research on other NFT projects in order to apply the model result broadly. 

- To apply the model convenience, we chose to make the dependent variable sold and not sold a binary variable, which ignored the number of sales. However, the buying behavior of crowd psychology might also affect the selling price and whether it has been sold. We will conduct future research on the variable affection for the number of sales. 


