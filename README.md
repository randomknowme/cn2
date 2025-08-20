https://github.com/randomknowme/cn1

https://github.com/randomknowme/cn2

https://github.com/randomknowme/cn3

# t1
``` python
from sklearn.datasets import load_iris 
import pandas as pd, numpy as np
from scipy.stats import boxcox, shapiro
import matplotlib.pyplot as plt
import seaborn as sns

# Load dataset
iris = load_iris()
df = pd.DataFrame(iris.data, columns=iris.feature_names)

# Features to test
features = ['sepal length (cm)', 'sepal width (cm)', 'petal length (cm)', 'petal width (cm)']

# Store results
results = {}
np.random.seed(42)

for feature in features:
    data = df[feature]
    # Apply transformations
    log_trans = np.log1p(data)
    sqrt_trans = np.sqrt(data)
    boxcox_trans, _ = boxcox(data + 1e-6)  # avoid zero values

    # Shapiro tests
    original_test = shapiro(data.sample(100))
    log_test = shapiro(pd.Series(log_trans).sample(100))
    sqrt_test = shapiro(pd.Series(sqrt_trans).sample(100))
    boxcox_test = shapiro(pd.Series(boxcox_trans).sample(100))

    results[feature] = {
        "Original": (original_test.statistic, original_test.pvalue),
        "Log": (log_test.statistic, log_test.pvalue),
        "Sqrt": (sqrt_test.statistic, sqrt_test.pvalue),
        "BoxCox": (boxcox_test.statistic, boxcox_test.pvalue)
    }

    # Plot histograms for each transformation
    fig, axes = plt.subplots(1, 4, figsize=(20, 4))
    sns.histplot(data, kde=True, ax=axes[0])
    axes[0].set_title(f"{feature} - Original")
    sns.histplot(log_trans, kde=True, ax=axes[1])
    axes[1].set_title("Log")
    sns.histplot(sqrt_trans, kde=True, ax=axes[2])
    axes[2].set_title("Sqrt")
    sns.histplot(boxcox_trans, kde=True, ax=axes[3])
    axes[3].set_title("Box-Cox")
    plt.suptitle(f"Distributions of {feature} with Transformations")
    plt.tight_layout(rect=[0, 0, 1, 0.95])
    plt.show()
results
```

# t2
```python
from sklearn.datasets import load_iris
import pandas as pd, seaborn as sns, matplotlib.pyplot as plt
iris = load_iris()
df = pd.DataFrame(iris.data, columns=iris.feature_names)
df["species"] = pd.Categorical.from_codes(iris.target, iris.target_names)
# Pair Plot
sns.pairplot(df, hue="species", diag_kind="hist")
plt.show()
# Box Plots
for col in iris.feature_names:
    sns.boxplot(x="species", y=col, data=df)
    plt.title(f"Boxplot: {col}")
    plt.show()
# Violin Plots
for col in iris.feature_names:
    sns.violinplot(x="species", y=col, data=df)
    plt.title(f"Violin Plot: {col}")
    plt.show()
# Correlation Heatmap
sns.heatmap(df.corr(numeric_only=True), annot=True, cmap="coolwarm")
plt.title("Correlation Heatmap")
plt.show()

```

# t4
```python
import numpy as np, matplotlib.pyplot as plt
from sklearn.datasets import load_iris
from sklearn.model_selection import train_test_split
from sklearn.linear_model import LogisticRegression
from sklearn.metrics import accuracy_score, confusion_matrix, roc_curve, roc_auc_score

# Load binary classification dataset (Setosa vs Versicolor)
iris = load_iris()
X = iris.data[iris.target != 2]
y = iris.target[iris.target != 2]
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=0)

# Gradient Descent based Logistic Regression
def sigmoid(z): return 1 / (1 + np.exp(-z))
w = np.zeros(X_train.shape[1])
b = 0
lr = 0.01
epochs = 2000

for _ in range(epochs):
    z = X_train @ w + b
    y_hat = sigmoid(z)
    error = y_hat - y_train
    w -= lr * X_train.T @ error / len(y_train)
    b -= lr * np.mean(error)

# Predict using GD
y_prob_gd = sigmoid(X_test @ w + b)
y_pred_gd = (y_prob_gd >= 0.5).astype(int)
acc_gd = accuracy_score(y_test, y_pred_gd)
conf_gd = confusion_matrix(y_test, y_pred_gd)
fpr_gd, tpr_gd, _ = roc_curve(y_test, y_prob_gd)
auc_gd = roc_auc_score(y_test, y_prob_gd)

# Sklearn Logistic Regression
model = LogisticRegression()
model.fit(X_train, y_train)
y_pred_lib = model.predict(X_test)
y_prob_lib = model.predict_proba(X_test)[:, 1]

acc_lib = accuracy_score(y_test, y_pred_lib)
conf_lib = confusion_matrix(y_test, y_pred_lib)
fpr_lib, tpr_lib, _ = roc_curve(y_test, y_prob_lib)
auc_lib = roc_auc_score(y_test, y_prob_lib)

# Results
print("Gradient Descent Accuracy:", acc_gd)
print("Gradient Descent Confusion Matrix:\n", conf_gd)
print("Gradient Descent AUC:", auc_gd)
print("\nLibrary Accuracy:", acc_lib)
print("Library Confusion Matrix:\n", conf_lib)
print("Library AUC:", auc_lib)

# ROC Curve Plot
plt.plot(fpr_gd, tpr_gd, label=f'GD AUC = {auc_gd:.2f}', linestyle='--')
plt.plot(fpr_lib, tpr_lib, label=f'Library AUC = {auc_lib:.2f}', linewidth=2)
plt.plot([0, 1], [0, 1], 'k--')
plt.xlabel("False Positive Rate")
plt.ylabel("True Positive Rate")
plt.title("ROC Curve: Gradient Descent vs Library")
plt.legend()
plt.grid()
plt.show()
```

# t5
```python
import numpy as np, matplotlib.pyplot as plt
from sklearn.datasets import load_iris
from sklearn.model_selection import train_test_split
from sklearn.linear_model import LogisticRegression
from sklearn.metrics import accuracy_score, confusion_matrix, roc_curve, roc_auc_score

# Load binary classification dataset (Setosa vs Versicolor)
iris = load_iris()
X = iris.data[iris.target != 2]
y = iris.target[iris.target != 2]
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=0)

# Gradient Descent based Logistic Regression
def sigmoid(z): return 1 / (1 + np.exp(-z))
w = np.zeros(X_train.shape[1])
b = 0
lr = 0.01
epochs = 2000

for _ in range(epochs):
    z = X_train @ w + b
    y_hat = sigmoid(z)
    error = y_hat - y_train
    w -= lr * X_train.T @ error / len(y_train)
    b -= lr * np.mean(error)

# Predict using GD
y_prob_gd = sigmoid(X_test @ w + b)
y_pred_gd = (y_prob_gd >= 0.5).astype(int)
acc_gd = accuracy_score(y_test, y_pred_gd)
conf_gd = confusion_matrix(y_test, y_pred_gd)
fpr_gd, tpr_gd, _ = roc_curve(y_test, y_prob_gd)
auc_gd = roc_auc_score(y_test, y_prob_gd)

# Sklearn Logistic Regression
model = LogisticRegression()
model.fit(X_train, y_train)
y_pred_lib = model.predict(X_test)
y_prob_lib = model.predict_proba(X_test)[:, 1]

acc_lib = accuracy_score(y_test, y_pred_lib)
conf_lib = confusion_matrix(y_test, y_pred_lib)
fpr_lib, tpr_lib, _ = roc_curve(y_test, y_prob_lib)
auc_lib = roc_auc_score(y_test, y_prob_lib)

# Results
print("Gradient Descent Accuracy:", acc_gd)
print("Gradient Descent Confusion Matrix:\n", conf_gd)
print("Gradient Descent AUC:", auc_gd)
print("\nLibrary Accuracy:", acc_lib)
print("Library Confusion Matrix:\n", conf_lib)
print("Library AUC:", auc_lib)

# ROC Curve Plot
plt.plot(fpr_gd, tpr_gd, label=f'GD AUC = {auc_gd:.2f}', linestyle='--')
plt.plot(fpr_lib, tpr_lib, label=f'Library AUC = {auc_lib:.2f}', linewidth=2)
plt.plot([0, 1], [0, 1], 'k--')
plt.xlabel("False Positive Rate")
plt.ylabel("True Positive Rate")
plt.title("ROC Curve: Gradient Descent vs Library")
plt.legend()
plt.grid()
plt.show()
```
https://github.com/randomknowme/cn1

https://github.com/randomknowme/cn2

https://github.com/randomknowme/cn3
