# Ex.No: 02 LINEAR AND POLYNOMIAL TREND ESTIMATION
Date:04-11-2026
### AIM:
To Implement Linear and Polynomial Trend Estiamtion Using Python.

### ALGORITHM:
Import necessary libraries (NumPy, Matplotlib)

Load the dataset

Calculate the linear trend values using least square method

Calculate the polynomial trend values using least square method

End the program
### PROGRAM:
```py
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt


data = pd.read_csv("user_behavior_timeseries.csv")
data.columns = data.columns.str.strip()


data['Date'] = pd.to_datetime(data['Date'])


col = "App Usage Time"
data[col] = data[col] + np.random.randint(-15, 15, size=len(data))


data = data.groupby('Date').mean(numeric_only=True)


Y = data[col].values
n = len(Y)

# Create centered X
X = np.arange(n)
X = X - n//2


x2 = X**2
xy = X * Y

b = (n*np.sum(xy) - np.sum(X)*np.sum(Y)) / (n*np.sum(x2) - (np.sum(X))**2)
a = (np.sum(Y) - b*np.sum(X)) / n

linear_trend = a + b*X

x3 = X**3
x4 = X**4
x2y = x2 * Y

# Normal equations matrix
A = np.array([
    [n, np.sum(X), np.sum(x2)],
    [np.sum(X), np.sum(x2), np.sum(x3)],
    [np.sum(x2), np.sum(x3), np.sum(x4)]
], dtype=np.float64)

B = np.array([
    np.sum(Y),
    np.sum(xy),
    np.sum(x2y)
], dtype=np.float64)

a_poly, b_poly, c_poly = np.linalg.solve(A, B)

poly_trend = a_poly + b_poly*X + c_poly*(X**2)


plt.figure(figsize=(8,10))

# Linear graph
plt.subplot(2,1,1)
plt.plot(data.index, Y, 'o-', label='Original Data')
plt.plot(data.index, linear_trend, '--', label='Linear Trend')
plt.title("Linear Trend Estimation")
plt.xlabel("Date")
plt.ylabel("Usage")
plt.legend()
plt.grid(True)

# Polynomial graph
plt.subplot(2,1,2)
plt.plot(data.index, Y, 'o-', label='Original Data')
plt.plot(data.index, poly_trend, '--', label='Polynomial Trend')
plt.title("Polynomial Trend Estimation (Degree 2)")
plt.xlabel("Date")
plt.ylabel("Usage")
plt.legend()
plt.grid(True)

plt.tight_layout()
plt.show()


print("FINAL EQUATIONS:")
print(f"Linear Trend: y = {a:.2f} + {b:.2f}x")
print(f"Polynomial Trend: y = {a_poly:.2f} + {b_poly:.2f}x + {c_poly:.2f}x²")
```
### OUTPUT
A - LINEAR TREND ESTIMATION

<img width="1140" height="630" alt="image" src="https://github.com/user-attachments/assets/5612ad9a-7099-4703-a065-a41d5506ccbc" />

B- POLYNOMIAL TREND ESTIMATION

<img width="1247" height="722" alt="image" src="https://github.com/user-attachments/assets/08ef55e2-cc32-45c4-afed-da4757d3cf96" />

### RESULT:
Thus the python program for linear and Polynomial Trend Estiamtion has been executed successfully.
