# Fulton fish market and the price elasticity of demand estimation

## **The Why**
With attractive price-setting at the front of their growth strategy, major eCommerce companies (Amazon, Zalando, BestSecret, etc.) consider a clean estimation of the price elasticities of their products *vital*. The price elasticity of demand informs us how much a price increase (or decrease) will impact the demand for a product. While these companies employ valuable customer datasets for the estimation of the price elasticities of their products, in this exercise, we will use a freely available dataset to revisit fundamental concepts and present a causal estimate of the price elasticity.

### Why price elasticity matters?
Many goods we consume are considered elastic, i.e., their demand goes up (down) when their prices increase (decrease). 
For example, luxury clothing and electronics consumption often react to price changes and hence are price 
elastic. In comparison, the consumption of food/medicines may not respond to price changes, so is considered price 
inelastic. Interestingly, it is noteworthy that not all consumer electronics are price elastic; e.g., consumer 
loyalty of iPhone users is well known and makes the demand of iPhones price inelastic. This underscores the 
importance of giving special attention to separately estimating the price elasticities of different products. 

### Why causal estimate of the price elasticity is needed?


## **The How**
While many advanced machine learning models for causal inference are readily available and relatively easy to implement, we will address the potential concern of endogeneity in $Price$ with the help of the instrumental variables (IV) technique. The choice of IV technique is largely driven by the ease of data access and is limited to demonstrating the methodology's main artifacts and implementation. For more information on the discussion and description of the variables and methodology shown here, please see the original article by Kathryn Graddy and compare your observations (it will be an excellent replication exercise).[[1]](#1). A cleaned version of her dataset is accessible on her homepage.[[2]](#2) 


### Modeling the price elasticity of demand
Mathematically speaking, the price elasticity of demand is simply a derivative of demanded quantity with respect to price. Formally, it is written as below:

$$e = \frac{dQ / Q}{dP / P}$$

In regression analysis, $e$ is often estimated using the log-log linear regression model of the $Price-Quantity$ relationship. That is, the price elasticity of demand ($e$) is simply the coefficient of the variable $log(Price)$, as shown below:

$$ \log{}Quantity = \beta_0 + e\log{}(Price) + \epsilon$$

### Endogeneity concerns
The price elasticity of demand ($e$) estimated above suffers from what economists call endogeneity, which comes in the way of interpreting $e$ causally. The endogeneity problem arises when the explanatory variable, in our case $Price$, is correlated with the error term $\epsilon$. 

There are broadly three reasons why the endogeneity issue arises: 1. omitted variables bias, 2. measurement error, 3. and simultaneity. For more information on the sources of endogeneity in price elasticity of demand estimation, please see the influential research by [Angrist and Krueger](https://pubs.aeaweb.org/doi/pdfplus/10.1257/jep.15.4.69).[[4]](#4) 

In the context of eCommerce, we can readily imagine the following two sources that can make $Price$ endogenous:
- **Seasonality:** The demand and supply of many products differ by season. Many products are sold at lower prices during off-seasons (demand is also low) and are sold at higher prices during holiday seasons, e.g., Christmas (high demand). To this end, this seasonality of the product market is a *confounder* that intervenes in the relationship of interest.
- **Product quality**: Product quality is another confounder that can determine $Price$ and the $Quantity sold$ of the product. For example, iPhones are generally assumed to have higher quality than Android phones. To this end, iPhones are also sold in higher quantities and at higher prices than many Android phones. A simple analysis of the retailer sales data then should suggest that higher prices are correlated with higher quantity sold, biasing the coefficient of the Price-Quantity relationship, i.e., $e$.

### Instrumental Variables algorithm of causal inference
We want to estimate the response of market demand to exogenous changes in market price. We do this by addressing the potential endogeneity in $Price$ by including an Instrumental Variable (IV) in the system of equation. The IV need to be correlated with $Price$, but should not directly affect the quantity demanded $Quantity$. It allows us to generate an exogenous variation *only* in the endogenous regressor ($Price$), facilitating the assessment of the impact of $Price$ on $Quantity$ of the product.  
Such variables are not always easily available and many times require us to conduct costly experiments. 

We use an instrumental variable (IV), a third variable, in the regression analysis when we have endogenous variables, e.g., $Price$. 
Among others, two main conditions need to be met for the IV to help us identify the causal influence of $Price$ on $Quantity$. 
1. The IV is highly correlated with the endogenous variable --> see first-stage regression.
2. The IV is uncorrelated with the error term $\epsilon$ and affects the dependent variable ($Quantity$) only through its influence on the endogenous variable --> no formal tests exist to demonstrate that this condition is met.

The estimation takes place in two steps:
1. First, we regress the IV variable on the endogenous regressor.  

$$ \log{}Price = \alpha_0 + \alpha_0(IV) + u$$

We obtain the predicted $Price$ from this regression, denoted as $\hat{Price}$.

2. In the quantity-price regression above, we then replace the endogenous regressor with its predicted value $\hat{Price}$. The estimated coefficient on the predicted $Price$ is our causal estimate for the price elasticity of demand.

$$ \log{}Quantity = \gamma_0 + e_c\log{}(\hat{Price}) + v$$

The $e_c$ in the above equation is our causal estimate of the price elasticity of demand.


## References
<a id="1">[1]</a> 
Graddy, K. (2006). 
Markets: The Fulton Fish Market. 
Journal of Economic Perspectives, 20 (2), 207-220.

<a id="2">[2]</a> 
https://www.kathryngraddy.org/research#pubdata

<a id="3">[3]</a> 
Youtube discussion: https://youtu.be/fpZC_tEfnLM

<a id="4">[4]</a> 
Angrist, JD and AB Krueger
Instrumental Variables and the Search for Identification: From Supply and Demand to Natural Experiments
Journal of Economic Perspectives, 15 (4), 69–85
