# Crypto-Coven-Analysis
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

In order to assess model fit, we applied a binned residual plot on our model which is located in the appendix. 


#### Linear Regression Model 

To answer the second question of what features are most strongly associated with sale price we used a linear regression model since our outcome variable is continuous.

We conducted a stepwise selection of AIC criterion to select the best linear regression model because 16 variables are not too many and BIC criterion is too powerful in this case. AIC provides a medium strength parameter penalty.   

After the AIC-selection, we choose our final linear regression model to determine what features are associated with the increase in NFT's price.  

* linear model assessment

In order to assess model fit, we applied 4 residual plot on our model to check if  model assumptions are being met or not met which is located in the appendix.  

# Results  

### EDA  

#### Plots for Soft Sklls count, Props Count, Total Price (Unit: Gwei) Count, Sold or Non-Sold 

```{r,fig.height=8.5,fig.width=12,warning=FALSE,message=FALSE,fig.align='center'}
plot3 = ggplot(nft_df, aes(x = soft_sklls)) +
  geom_bar() +
  ggtitle("Barplot for Soft Sklls Count") +
  xlab('soft skills') +
  geom_text(stat='count', aes(label=..count..), vjust=-0.5,size = 3) +
  theme(axis.text.x = element_text(angle = 90, vjust = 0.5, hjust=1), 
        plot.title = element_text(hjust = 0.5))
plot4 = ggplot(nft_df, aes(x = props)) +
  geom_bar() +
  ggtitle("Barplot for Props Count") +
  geom_text(stat='count', aes(label=..count..), vjust=-0.5,size = 3) +
  theme(axis.text.x = element_text(angle = 90, vjust = 0.5, hjust=1), 
        plot.title = element_text(hjust = 0.5))

plot1 = ggplot(nft_df, aes(x = last_sale.total_price)) +
  geom_freqpoly() +
  ggtitle("Freqpoly Plot for Total Price (Unit: Gwei) Count") +
  xlab('total price') +
  geom_text(stat='count', aes(label=..count..), vjust=-0.5,size = 3) +
  theme(axis.text.x = element_text(angle = 90, vjust = 0.5, hjust=1), 
        plot.title = element_text(hjust = 0.5))

plot2 = ggplot(nft_df, aes(x = sale_status)) +
  geom_bar() +
  ggtitle("Barplot for Sold(1) or Not(0)") +
  xlab('sale status') +
  geom_text(stat='count', aes(label=..count..), vjust=-0.5,size = 3) +
  theme(axis.text.x = element_text(angle = 90, vjust = 0.5, hjust=1), 
        plot.title = element_text(hjust = 0.5))
grid.arrange(plot1, plot2, plot3, plot4, ncol=2, nrow = 2)

```


