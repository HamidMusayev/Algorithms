### **Random Forest 🤖**

An **ensemble of decision trees** that reduces variance by:
1. **Bagging:** train each tree on a bootstrap sample of the data.
2. **Feature subsampling:** at each split, consider only a random subset of features.
3. **Vote / average:** combine predictions across trees.

---

#### **Summary**
- **Train time:** O(T · n · d · log n) for T trees
- **Predict time:** O(T · depth)
- **Strengths:** robust, less overfitting than a single tree, handles non-linear data
- **Weaknesses:** larger model, less interpretable

---

#### **Python Implementation (scikit-learn)**
```python
from sklearn.ensemble import RandomForestClassifier
from sklearn.datasets import load_iris
from sklearn.model_selection import train_test_split

X, y = load_iris(return_X_y=True)
X_tr, X_te, y_tr, y_te = train_test_split(X, y, random_state=0)

rf = RandomForestClassifier(n_estimators=200, max_depth=None, random_state=0)
rf.fit(X_tr, y_tr)

print("Accuracy:", rf.score(X_te, y_te))
print("Feature importances:", rf.feature_importances_)
```

---

#### **Key Hyperparameters**
- `n_estimators` — more trees → less variance, more cost.
- `max_depth` / `min_samples_leaf` — control overfitting.
- `max_features` — `'sqrt'` (classification) or `'log2'` are common defaults.

---

#### **When to Use**
✅ Strong baseline for tabular data
✅ When you want feature importances "for free"
🚫 Very large datasets → consider Gradient Boosted Trees (XGBoost, LightGBM, CatBoost)
