# Determinants of Passenger Demand in Sri Lanka's Civil Aviation Sector


Passenger demand is one of the most important indicators influencing the performance and sustainability of the civil aviation industry. It determines airline profitability, airport capacity utilization, infrastructure investments, and the effectiveness of transportation planning. For an island nation such as Sri Lanka, where international connectivity plays a significant role in tourism, trade, and economic development, understanding the factors that influence passenger demand is essential for evidence-based decision-making. The Civil Aviation Authority of Sri Lanka, Airport and Aviation Services (Sri Lanka) Ltd., and commercial airlines require reliable statistical evidence to forecast future passenger volumes and optimize operational strategies. The present analysis investigates the key determinants of passenger demand using the Air_Transport_Data.csv dataset, which consists of 200 observations representing air transport routes and operational characteristics.

  

The dependent variable in this study is passenger demand, while the independent variables include airport traffic, average income, fuel price, average ticket fare, flight frequency, and route distance. These variables were selected because they represent operational, economic, and market conditions that are commonly associated with air travel demand. Airport traffic reflects the overall activity level of an airport and often indicates the availability of services and passenger connectivity. Average income represents the purchasing power of consumers and their ability to afford air travel. Fuel price affects airline operating costs and can indirectly influence ticket prices and passenger demand. Average ticket fare directly determines the affordability of air travel, whereas flight frequency measures service availability and convenience. Route distance captures the geographical characteristics of each route, which may influence passenger preferences and airline operating decisions.

  

## Normality Testing and Hypothesis Development

  

Before examining the relationships between the independent variables and passenger demand, it is essential to determine whether the dataset satisfies the assumptions required for parametric statistical analysis. Normality testing evaluates whether the distribution of each variable approximates a normal distribution, which is a fundamental assumption for Pearson correlation analysis and linear regression modelling. If the data are normally distributed, the resulting statistical estimates are considered more reliable and the significance tests become more robust. Consequently, testing for normality provides the foundation for selecting appropriate analytical techniques and ensures that the conclusions drawn from the regression models are statistically valid.

  

To assess normality, formal hypothesis testing was conducted for each variable in the dataset. The hypotheses were defined as follows:

  

- **Null Hypothesis (H₀):** The variable follows a normal distribution.

- **Alternative Hypothesis (H₁):** The variable does not follow a normal distribution.

  

The decision criterion was based on a significance level of α = 0.05. A p-value greater than 0.05 indicates insufficient evidence to reject the null hypothesis, suggesting that the variable is approximately normally distributed. Conversely, a p-value less than 0.05 indicates that the distribution significantly deviates from normality.

  

The results of the normality analysis indicate that the variables included in the dataset satisfy the assumptions required for parametric analysis, allowing the study to proceed with Pearson correlation and linear regression techniques. Although minor deviations from perfect normality are expected in real-world transportation data due to operational fluctuations and market dynamics, these deviations were not substantial enough to compromise the validity of the subsequent statistical analyses. This finding is particularly important because the dataset contains 200 observations, and according to the Central Limit Theorem, larger sample sizes generally reduce the impact of slight departures from normality on regression estimates.

  

In addition to formal hypothesis testing, graphical methods such as histograms and Q-Q plots (Normal Quantile-Quantile plots) can be used to visually assess whether the observed data closely follow a theoretical normal distribution. Variables that exhibit approximately bell-shaped histograms and Q-Q plots with points closely aligned along the reference line provide further evidence that the normality assumption has been satisfied. Visual inspection therefore complements the statistical tests by confirming that no extreme skewness or irregular patterns are present in the dataset.

  

Establishing normality is an important step because it directly influences the choice of statistical methods used in the remainder of the analysis. Since the data satisfy the assumptions of normality, Pearson's correlation coefficient is appropriate for measuring the strength and direction of linear relationships between passenger demand and the explanatory variables. Similarly, both Simple Linear Regression (SLR) and Multiple Linear Regression (MLR) can be applied confidently to estimate predictive relationships without introducing substantial bias resulting from non-normal data distributions.

  

Following the confirmation of normality, the analysis proceeds to investigate the degree of association between passenger demand and each independent variable. Correlation analysis serves as the first stage in understanding these relationships by identifying whether increases or decreases in variables such as airport traffic, average income, flight frequency, fuel price, average ticket fare, and route distance are associated with corresponding changes in passenger demand. These findings provide the statistical foundation for the regression models developed in the subsequent sections, ensuring that the predictive analysis is supported by meaningful and statistically significant relationships rather than random variation.

  

## Correlation Analysis

  

Following the confirmation that the data satisfy the assumptions of normality, the next stage of the analysis examines the relationships between the independent variables and passenger demand through correlation analysis. Correlation analysis is an important preliminary statistical technique because it measures both the strength and direction of the association between two variables before developing predictive regression models. By identifying whether variables move together positively or negatively, the analysis provides an initial understanding of which factors are likely to influence passenger demand within Sri Lanka's civil aviation sector.

  

To evaluate these relationships, hypothesis testing was performed using correlation coefficients. The hypotheses for each independent variable were formulated as follows:

  

- **Null Hypothesis (H₀):** There is no statistically significant relationship between the independent variable and passenger demand (ρ = 0).

- **Alternative Hypothesis (H₁):** There is a statistically significant relationship between the independent variable and passenger demand (ρ ≠ 0).

  

A significance level of α = 0.05 was adopted throughout the analysis. Variables with p-values less than 0.05 were considered to have statistically significant relationships with passenger demand, while variables with p-values greater than 0.05 indicated insufficient evidence to reject the null hypothesis.

  

Since the normality assumption had already been verified, Pearson's correlation coefficient was used as the primary measure of linear association. Pearson's correlation coefficient ranges from -1 to +1, where values closer to +1 indicate a strong positive relationship, values closer to -1 indicate a strong negative relationship, and values near zero suggest little or no linear relationship. To complement the Pearson analysis, Spearman's rank correlation coefficient was also examined to verify that the observed relationships remained consistent even when the variables were ranked rather than measured on their original scales. Using both measures increases confidence in the reliability of the findings, particularly when analysing transportation datasets that may contain slight variations or outliers.

  

The correlation results demonstrate that airport traffic exhibits a strong positive relationship with passenger demand. This finding is expected because airports handling higher volumes of aircraft movements and passengers generally offer greater connectivity, more flight options, and improved accessibility, all of which encourage higher passenger demand. Similarly, flight frequency shows a positive association with passenger demand, indicating that routes with more frequent services are generally more attractive to travellers due to increased flexibility and reduced waiting times. These findings suggest that operational factors play a significant role in determining passenger volumes.

  

The analysis also indicates that average income is positively correlated with passenger demand. As household income increases, consumers possess greater purchasing power and are more capable of affording air travel for business, tourism, and personal purposes. This relationship supports established economic theory, which identifies income as one of the primary determinants of travel demand. In contrast, average ticket fare demonstrates a negative relationship with passenger demand. Higher airfares increase the cost of travel and reduce affordability, discouraging some passengers from choosing air transportation. Likewise, fuel price shows a negative association with passenger demand because increases in fuel costs often lead to higher operating expenses for airlines, which are frequently passed on to passengers through increased ticket prices.

  

The relationship between route distance and passenger demand appears comparatively weaker than those observed for the other variables. While longer routes may generate demand for international travel, they also involve higher operating costs and often result in higher ticket prices, producing a more complex relationship with passenger demand. This suggests that route distance alone may not be a strong predictor unless considered alongside other operational and economic factors within a multiple regression framework.

  

To support the statistical findings, scatter plots were generated for each independent variable against passenger demand. These graphical representations provide a visual assessment of the relationships identified through the correlation coefficients. Scatter plots for airport traffic, average income, and flight frequency display clear upward trends, confirming positive linear relationships with passenger demand. Conversely, scatter plots for average ticket fare and fuel price exhibit downward trends, illustrating their inverse relationships with passenger demand. The scatter plot for route distance presents a more dispersed pattern, supporting the conclusion that its individual relationship with passenger demand is comparatively weaker. Overall, the absence of pronounced nonlinear patterns indicates that linear regression is an appropriate modelling technique for the subsequent stages of the analysis.

  

The findings from the correlation analysis provide the statistical justification for proceeding to regression modelling. Although correlation identifies whether variables are associated with passenger demand, it does not quantify the extent to which each variable predicts changes in passenger demand. Consequently, the next stage employs Simple Linear Regression (SLR) to evaluate the individual predictive effect of each independent variable before constructing a comprehensive Multiple Linear Regression (MLR) model that incorporates all predictors simultaneously.