# Volatility-Forecasting-
University assignment to create, test, and justify predictive models to analyse Coca Cola's volatility on real world stock data

Introduction

The Coca-Cola Company (KO) is a beverage company founded in 1886 and headquartered in Atlanta, Georgia. Coca-Cola manufactures and sells various non-alcoholic beverages globally. 
The company produces a wide variety of beverages, including sparkling soft drinks, flavours, coffees, teas, juices and more. It also offers concentrates and syrups for retailers such as restaurants and convenience stores. 
The company sells its products under a variety of names and brands and operates through a network of bottling partners, distributors, wholesalers, and retailers covering most, if not all aspects of the beverage sector.



Basic statistics for the returns plot of KO:

 ![Returns](https://github.com/user-attachments/assets/8a511137-bbd6-40bf-a17d-1e6160dd1e82)

Mean 0.027
Variance 1.633
Skewness -0.775

Kurtosis 8.840





Model Description:

Selected models are ARCH(1), GARCH(1,1) and GJR GARCH. I decided to go with these models:

ARCH(1) – simple baseline model, a good model to determine if more complex models will be required

GARCH(1,1) – Coca-Cola has a long track record, and GARCH will allow the capture of shocks, as well as ensuring they have a realistic tail (introduces the concept of Lag), ensuring historical data doesn’t have too large an impact on forecasts

GJR GARCH (1,1) – Introduces leverage, the idea that negative shocks increase volatility more than positive shocks. This may improve the model's one-step-ahead forecast. 

All these models are also selected for their relatively low computational strain, parsimony and decreased risk of over fitting the sample data



ARCH (1) model: 
r_t=0.11+ε_t
σ_t^2=1.34+0.43ε_(t-1)^2
ε_t=σ_t z_t    z~N(0,1)


![vol_arch](https://github.com/user-attachments/assets/24e29b9c-8d72-4745-bc75-433e9aa52fde)


GARCH(1,1) model:
r_t=0.06+ε_t
σ_t^2=0.06+0.11ε_(t-1)^2+〖0.86σ〗_(t-1)^2
ε_t=σ_t z_t    z~N(0,1)


![garch_vol](https://github.com/user-attachments/assets/46078c3e-56bd-4305-8c17-008de90058e5)



GJR GARCH (1,1) model:
r_t=0.04+ε_t
σ_t^2=0.06+(0.06+0.08*{ε_t<0})ε_(t-1)^2+〖0.86σ〗_(t-1)^2
ε_t=σ_t z_t    z~N(0,1)


![gjr_vol](https://github.com/user-attachments/assets/1a4e565f-c1da-4e60-81e7-22c72d0da876)




Model Comparison:
	            AIC	    BIC	    LOGLIKLIHOOD
ARCH(1)	      3.469	  3.487    -1304.787
GARCH(1,1)    3.263	  3.288	   -1226.367
GJR – GARCH	  3.261	  3.292	   -1224.524

Based on the above measures, it is clear that in-sample fit using the GARCH and GJR GARCH models is likely to yield a more accurate one-day-ahead forecast, with a lower BIC, AIC, and log likelihoods over the ARCH(1) model. These results make sense, with GARCH introducing lag and GJR introducing the Leverage effect. 
Volatility analysis

For the forecasting conducted below for the GARCH(1,1) and the GJR GARCH(1,1) models, I have decided to use a rolling window. I have chosen this as it is both computationally efficient and only provides weight to recent shocks in volatility.

![gjr_roll](https://github.com/user-attachments/assets/fc647d1c-ea21-494e-afa5-850d4d06b15c)

![gar_roll](https://github.com/user-attachments/assets/a7ae538d-7c11-4582-b705-364ffaa7ebd5)

Results of the one-step-ahead forecasts are:
GJR GARCH –  1.23
GARCH (1,1) – 1.14

The spike in volatility around September 2023 may be the result of a strong Q3 report and a potential surge in stock prices. This might be the result of investors taking an interest in a large, established company performing well within an environment filled with concerns around post-COVID economic inflation and rising interest rates. 

The rise in volatility around early to mid-2025 may be a result of Donald Trump’s trade war and economic sanctions, creating uncertainty around multinational companies and international supply chains. 

When examining the one-step-ahead forecasts and out-of-sample predictions of both models, they are very similar. Their one-step ahead volatility predictions are 0.9 apart, and their accuracy measures, MSE and Squared loss, are extremely similar, with GARCH(1,1) performing marginally better. 

MSE for GJR GARCH - 3.907

![se_gjr_roll](https://github.com/user-attachments/assets/d73eb4aa-edbc-4940-9146-518dc850d938)


MSE for GARCH(1,1) - 3.871

![SE_gar_roll](https://github.com/user-attachments/assets/16c656b5-3911-409b-8093-10b94a78aab0)


