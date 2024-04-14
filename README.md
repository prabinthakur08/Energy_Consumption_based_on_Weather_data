# Energy_Consumption_based_on_Weather_data

This is a group project to develop a model that predicts the energy consumption based on weather data.

## Team members:

Bronwyn Gardiner

Prabin Thakur

Jenna Wong Kai Pun

Yudong Zheng


## For this project we have consider following scenario:

### For weather data:
Columns: direction_of_maximum_wind_gust, speed_of_maximum_wind_gust(km/h) and time_of_maximum_wind_gust have missing value which were replaced by 'NaN', 0 and '0:00' respectively.

Also 9am_wind_speed(km/h) and 3pm_wind_speed(km/h) has a string value of 'Calm' meaning complete absence, which is replace by 0 and it's data type is converted to float to maintain consistency. Similarly, date is converted to python datetime from string type and 3 new variables: month, weekday and type_of_week is created.

Following variables were rename accordingly:

"date" : "date_only", "minimum_temperature(°c)" : "min_temp", 
"maximum_temperature(°c)" : "max_temp", "rainfall(mm)" : "rainfall", 
"evaporation(mm)" : "evaporation", "sunshine(hours)" : "sunshine", 
"direction_of_maximum_wind_gust" : "max_wind_dir", 
"speed_of_maximum_wind_gust(km/h)" : "max_wind_speed", 
"time_of_maximum_wind_gust" : "max_wind_time", 
"9am_temperature(°c)":"9am_temp", "9am_relative_humidity(%)" : "9am_rela_humi", 
"9am_cloud_amount(oktas)" : "9am_cloud", "9am_wind_direction" : "9am_wind_dir", 
"9am_wind_speed(km/h)" : "9am_wind_speed", "9am_msl_pressure(hpa)" : "9am_msl_press", 
"3pm_temperature(°c)" : "3pm_temp", "3pm_relative_humidity(%)" : "3pm_rela_humi", 
"3pm_cloud_amount(oktas)" : "3pm_cloud", "3pm_wind_direction" : "3pm_wind_dir", 
"3pm_wind_speed(km/h)" : "3pm_wind_speed", "3pm_msl_pressure(hpa)" : "3pm_msl_press"

### For Price demand data:
Raw aggregated data was downloaded from the Australian Energy Market Operator and recommended retail price (RRP) was added to the price and demand dataset. Variable SETTLEMENTDATE was converted to datetime and 2 new variables: date_only and time_only is created.

After data cleaning, both dataset is merged into a dataframe: merged_data.

### To create the best model: 

We have selected 2 option Decision Tree(DT) model and KNN(K Nearest Neighbour) Algorithm, because the nature of the problem is Classification and the Dependent variable is Categorical and independent varible is both categorical and continuous, mostly continuous.

### Decision Tree:
The initial DT is build with random state = 42 and 6 being the max depth as below:
dt = DecisionTreeClassifier(criterion="entropy", random_state=42, max_depth=6)

Later, we have looped the value of max_depth(dt_depth) from 2 to 15, the obtain the best depth, being 4. 

Similarly, to select the best features, we have applied filter based selection using:
feature_selector = SelectKBest(chi2, k=no_fea_selec), where number of feature(no_fea_selec) range from 3 to 13.
dt = DecisionTreeClassifier(criterion="entropy", random_state=42, max_depth=dt_depth)

Finally, we have validated the DT model with best features and best max_depth value using k-fold cross validation, running the value of k from 1 to 10.
features = ['rainfall', 'evaporation', '9am_rela_humi', '3pm_wind_speed', 'weekday', 'demand_max_cat_average']
kf = KFold(n_splits=10, shuffle=True, random_state=42)
dt = DecisionTreeClassifier(criterion="entropy", random_state=42, max_depth=4)

### K Nearest Neighbors Model:
The initial KNN model is build with random state = 42 and n_neighbors = 5, as below:
x_train, x_test, y_train, y_test = train_test_split(x, y, test_size=0.2, random_state=42)
knn = neighbors.KNeighborsClassifier(n_neighbors=5)

Similar to DT, to select the best features, we have applied filter based selection using:
feature_selector = SelectKBest(chi2, k=no_fea_selec), where number of feature(no_fea_selec) range from 3 to 13.
knn = neighbors.KNeighborsClassifier(n_neighbors=5)

Finally, we have validated the KNN model with best features and best max_depth value using k-fold cross validation, running the value of k from 1 to 10.
features = ['min_temp', 'evaporation', '9am_rela_humi', '9am_wind_dir_n', '3pm_cloud', '3pm_wind_speed', 'weekday', 'demand_max_cat_average']
kf = KFold(n_splits=10, shuffle=True, random_state=42)
knn = neighbors.KNeighborsClassifier(n_neighbors=neighbours), where neighbours value ranges from 1 to 10.

### Conclusion
After evaluating both the model and performing the validation, KNN model outperforms Decision Tree model, with heighest accuracy score.
