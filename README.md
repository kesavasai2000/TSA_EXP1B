# Ex.No: 1B                     CONVERSION OF NON STATIONARY TO STATIONARY DATA
# Date: 19-08-2025
## Name: K KESAVA SAI
## Register Number: 212223230105

### AIM:
To perform regular differncing,seasonal adjustment and log transformatio on international airline passenger data
### ALGORITHM:
1. Import the required packages like pandas and numpy
2. Read the data using the pandas
3. Perform the data preprocessing if needed and apply regular differncing,seasonal adjustment,log transformation.
4. Plot the data according to need, before and after regular differncing,seasonal adjustment,log transformation.
5. Display the overall results.
### PROGRAM:
```py
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
from statsmodels.tsa.seasonal import seasonal_decompose

# Load dataset
data = pd.read_csv("Clean_Dataset.csv")

# Group by days_left (treat it as time axis)
data = data.groupby('days_left')['price'].mean().reset_index()
data = data.sort_values('days_left')
data.set_index('days_left', inplace=True)

# Regular differencing
data['price_diff'] = data['price'] - data['price'].shift(1)

# Seasonal decomposition (use smaller period like 7 instead of 52, since days_left <= 49)
result = seasonal_decompose(data['price'], model='additive', period=7)
data['price_sea_diff'] = result.resid

# Log transformation
data['price_log'] = np.log(data['price'])

# Log differencing
data['price_log_diff'] = data['price_log'] - data['price_log'].shift(1)

# Seasonal decomposition on log-diff series
result_log = seasonal_decompose(data['price_log_diff'].dropna(), model='additive', period=7)
resid_log = result_log.resid
resid_log.index = data['price_log_diff'].dropna().index
data['price_log_seasonal_diff'] = resid_log

# Plotting
plt.figure(figsize=(16, 16))

plt.subplot(6, 1, 1)
plt.plot(data['price'], label='Original')
plt.legend(loc='best')
plt.title('Original Data')
plt.xlabel('Days Left')
plt.ylabel('Price')

plt.subplot(6, 1, 2)
plt.plot(data['price_diff'], label='Regular Difference')
plt.legend(loc='best')
plt.title('Regular Differencing')
plt.xlabel('Days Left')
plt.ylabel('Differenced Price')

plt.subplot(6, 1, 3)
plt.plot(data['price_sea_diff'], label='Seasonal Adjustment')
plt.legend(loc='best')
plt.title('Seasonal Adjustment')
plt.xlabel('Days Left')
plt.ylabel('Seasonally Adjusted Price')

plt.subplot(6, 1, 4)
plt.plot(data['price_log'], label='Log Transformation')
plt.legend(loc='best')
plt.title('Log Transformation')
plt.xlabel('Days Left')
plt.ylabel('Log(Price)')

plt.subplot(6, 1, 5)
plt.plot(data['price_log_diff'], label='Log Transformation and Regular Differencing')
plt.legend(loc='best')
plt.title('Log Transformation and Regular Differencing')
plt.xlabel('Days Left')
plt.ylabel('RDiff(Log(Price))')

plt.subplot(6, 1, 6)
plt.plot(data['price_log_seasonal_diff'], label='Log Transformation + Regular Differencing + Seasonal Differencing')
plt.legend(loc='best')
plt.title('Log Transformation + Regular Differencing + Seasonal Differencing')
plt.xlabel('Days Left')
plt.ylabel('SDiff(RDiff(Log(Price)))')

plt.tight_layout()
plt.show()

```

### OUTPUT:
## ORIGINAL DATA:
<img width="1429" height="244" alt="image" src="https://github.com/user-attachments/assets/a86d747d-8e06-4d83-a14d-b11594b52c4e" />

REGULAR DIFFERENCING:
<img width="1826" height="282" alt="image" src="https://github.com/user-attachments/assets/2d56804c-02fd-4c1d-bd91-d89d272bd864" />


SEASONAL ADJUSTMENT:
<img width="1836" height="289" alt="image" src="https://github.com/user-attachments/assets/6989932a-74b3-4569-9115-54b0340dfa59" />


LOG TRANSFORMATION:
<img width="1805" height="284" alt="image" src="https://github.com/user-attachments/assets/8b839043-d1e9-41e2-9a1c-a69b85962ad8" />

LOG TRANSFORMATION & REGULAR DIFFERENCING:
<img width="1797" height="292" alt="image" src="https://github.com/user-attachments/assets/55b2beb4-4ff2-4d55-9392-e4e1cce30356" />

LOG TRANSFORMATION + REGULAR DIFFERENCING + SEASONAL DIFFERENCING:
<img width="1794" height="307" alt="image" src="https://github.com/user-attachments/assets/214abcc2-9152-427f-b82e-76e18055b8e2" />


### RESULT:
Thus we have created the python code for the conversion of non stationary to stationary data on international airline passenger
data.
