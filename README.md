# Ex.No: 02 LINEAR AND POLYNOMIAL TREND ESTIMATION
### Date: 06-11-2026
### Reg no : 212223240162
### Dataset : Nvidia_stock price 
### AIM:
To Implement Linear and Polynomial Trend Estiamtion Using Python.

### ALGORITHM:
Import necessary libraries (NumPy, Matplotlib)

Load the dataset

Calculate the linear trend values using least square method

Calculate the polynomial trend values using least square method

End the program
### PROGRAM:
```

import pandas as pd
import numpy as np
import matplotlib.pyplot as plt

# Read Dataset
data = pd.read_csv('/content/NVIDIA_STOCK.csv')
data = data.iloc[2:].copy()

data['Price'] = pd.to_datetime(data['Price'])
data['Close'] = pd.to_numeric(data['Close'], errors='coerce')

data = data.dropna(subset=['Price', 'Close'])
data.set_index('Price', inplace=True)

# Annual Average Stock Price
resampled_data = data['Close'].resample('YE').mean().to_frame()
resampled_data.index = resampled_data.index.year
resampled_data.reset_index(inplace=True)
resampled_data.rename(columns={'Price': 'Year'}, inplace=True)

years = resampled_data['Year'].tolist()
prices = resampled_data['Close'].tolist()

# Linear Trend Estimation
X = [i - years[len(years)//2] for i in years]
x2 = [i**2 for i in X]
xy = [i*j for i, j in zip(X, prices)]

n = len(years)
b = (n*sum(xy) - sum(prices)*sum(X)) / (n*sum(x2) - sum(X)**2)
a = (sum(prices) - b*sum(X)) / n

linear_trend = [a + b*i for i in X]

# Polynomial Trend Estimation
x3 = [i**3 for i in X]
x4 = [i**4 for i in X]
x2y = [i*j for i, j in zip(x2, prices)]

coeff = [
    [n, sum(X), sum(x2)],
    [sum(X), sum(x2), sum(x3)],
    [sum(x2), sum(x3), sum(x4)]
]

Y = [sum(prices), sum(xy), sum(x2y)]

solution = np.linalg.solve(np.array(coeff), np.array(Y))
a_poly, b_poly, c_poly = solution

poly_trend = [
    a_poly + b_poly*i + c_poly*i**2 for i in X
]

# Display Results
print(f"Linear Trend: y = {a:.2f} + {b:.2f}x")
print(f"Polynomial Trend: y = {a_poly:.2f} + {b_poly:.2f}x + {c_poly:.2f}x²")

resampled_data['Linear Trend'] = linear_trend
resampled_data['Polynomial Trend'] = poly_trend

resampled_data.set_index('Year', inplace=True)

# Linear Trend Plot
resampled_data['Close'].plot(
    color='blue', marker='o', label='Actual Stock Price'
)
resampled_data['Linear Trend'].plot(
    color='black', linestyle='--', label='Linear Trend'
)

plt.title('NVIDIA Stock - Linear Trend')
plt.xlabel('Year')
plt.ylabel('Average Closing Price')
plt.legend()
plt.show()

# Polynomial Trend Plot
resampled_data['Close'].plot(
    color='blue', marker='o', label='Actual Stock Price'
)
resampled_data['Polynomial Trend'].plot(
    color='black', marker='o', label='Polynomial Trend'
)

plt.title('NVIDIA Stock - Polynomial Trend')
plt.xlabel('Year')
plt.ylabel('Average Closing Price')
plt.legend()
plt.show()

```

### OUTPUT

### LINEAR TREND ESTIMATION

<img width="571" height="455" alt="image" src="https://github.com/user-attachments/assets/0bfb8169-2720-43af-9ae3-88cd9c5be9ad" />

### POLYNOMIAL TREND ESTIMATION

<img width="571" height="455" alt="image" src="https://github.com/user-attachments/assets/8ba670ac-cccd-4be4-9b0f-991e42a095d6" />


### RESULT:
Thus the python program for linear and Polynomial Trend Estiamtion has been executed successfully.
