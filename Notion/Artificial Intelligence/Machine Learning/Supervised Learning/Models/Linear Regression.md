  

---

# 📦 **1. Linear Regression**

### 🧠 Fundamental Idea:

Linear regression models the relationship between input features (X) and a continuous target variable (y) by fitting a **line (or hyperplane)** that minimizes the difference between actual and predicted values.

---

### 🧮 Mathematical Foundation:

The hypothesis is:

$$\hat{y} = w_0 + w_1x_1 + w_2x_2 + \dots + w_nx_n = \mathbf{w}^\top \mathbf{x}$$

Where:

- $\hat{y}$: predicted output
- x: input vector
- w: weight vector
- $w_0$: bias (intercept term)

Objective: Minimize the **Mean Squared Error (MSE)**:

$$J(\mathbf{w}) = \frac{1}{n} \sum_{i=1}^{n} (y_i - \hat{y}_i)^2$$

We solve this by either:

- **Normal equation** (closed form)
- **Gradient descent** (iterative optimization)

---

### 📌 When to Use:

- Predicting continuous outcomes
- Interpretability matters
- Linearity assumption is reasonable
- You want a baseline model

---

### 🐍 Python Example:

```Python
from sklearn.linear_model import LinearRegression
from sklearn.model_selection import train_test_split
from sklearn.metrics import mean_squared_error
import numpy as np

# Dummy data
X = np.array([[1], [2], [3], [4], [5]])
y = np.array([2, 4, 6, 8, 10])

# Train/test split
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2)

# Model training
model = LinearRegression()
model.fit(X_train, y_train)

# Prediction
y_pred = model.predict(X_test)

# Evaluation
print("MSE:", mean_squared_error(y_test, y_pred))
print("Coefficients:", model.coef_)
print("Intercept:", model.intercept_)
```

---

### 🔧 Training & Interpretation:

- Training is done by minimizing the squared error.
- Coefficients tell you **how much each feature contributes** to the prediction.
- Intercept is the baseline value when all features are 0.

---

Would you like me to continue with the next model: **Logistic Regression** (for classification)? Or do you want to jump to another one like **Random Forest**, **SVM**, or **Neural Networks**?