# Predicting_NYC_Taxi_Gratuities
Predicting Taxi Gratuities in NYC

Project Overview

The goal of this project was to create a multiple linear regression, random forest model, and XGBoost model to predict high rider gratuity or not. This project utilized yellow taxi trips taken in New York City during 2017. The final XGBoost model performed with 83.2% accuracy and 82.3% precision determining what features were most important in separating low tippers from high tippers. Based on the model, the VendorID, fare amount, and cost of the trip were most influential in determining a generous tipper (>20%) vs a non-generous one (<20%). 

Business Understanding 

According to salary.com the average salary for a New York Taxi Driver is around $45,000. This salary is significantly low compared to a median rent value of $6,500 per month. It is important to understand what factors encourage riders to leave tips in order to help drivers obtain a livable wage. 

Data Understanding 

The NYC Taxi and Limousine Commission data came from NYC.gov. The data consisted of approximately 408k unique trips and 18 features. The features included information on trip duration and destination, vendor used, toll information, and payment type. The bar chart below shows the breakdown of how many generous tippers (>20%) versus non-generous tippers that exist in the data set. <img width="394" alt="Screenshot 2024-12-31 at 3 01 15 AM" src="https://github.com/user-attachments/assets/5e8b4ce2-0fa8-49fb-aa17-d3b20694439f" />


Modeling and Evaluation 

An XG Boost model comprising 100 decision trees was used to determine feature importance in who would tip generously or not. The below plot shows that VendorID, fare amount, and the total cost of a trip were the Top 3 most important factors in determining a generous tipper from a non-generous one. The overall model performed with 83.2% accuracy and 82.3% precision. <img width="915" alt="Screenshot 2024-12-31 at 3 06 14 AM" src="https://github.com/user-attachments/assets/febb5a57-3300-4dbc-9a27-e3cf23ffb6b9" />


Conclusion

This model can benefit Taxi Drivers in knowing if they will be tipped generously or not. In the future, adding more information on a rider’s past tipping behavior may also be beneficial in helping the stakeholder address their business problem. 

