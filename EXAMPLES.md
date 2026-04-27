# EXAMPLES.md

Before/after examples of AI-generated code, with and without `clinical-code-guidelines` applied.

---

## 1. Reproducibility First

### Without guidelines

```python
from sklearn.model_selection import train_test_split
from sklearn.ensemble import RandomForestClassifier

X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2)

model = RandomForestClassifier(n_estimators=100)
model.fit(X_train, y_train)

print(f"Accuracy: {model.score(X_test, y_test)}")
```

Every run produces a different split, a different model, a different result. No record of what data was used, what library version ran it, or what parameters produced the output.

### With guidelines

```python
import random
import numpy as np
import sklearn
from sklearn.model_selection import train_test_split
from sklearn.ensemble import RandomForestClassifier
from datetime import datetime

RANDOM_SEED = 42
random.seed(RANDOM_SEED)
np.random.seed(RANDOM_SEED)

run_log = {
    "timestamp": datetime.utcnow().isoformat(),
    "sklearn_version": sklearn.__version__,
    "random_seed": RANDOM_SEED,
    "data_source": "HFEA_registry_v2_cleaned.csv",
    "n_samples": len(X),
    "n_features": X.shape[1],
}
print("Run:", run_log)

X_train, X_test, y_train, y_test = train_test_split(
    X, y,
    test_size=0.2,
    random_state=RANDOM_SEED,
    stratify=y
)

model = RandomForestClassifier(n_estimators=100, random_state=RANDOM_SEED)
model.fit(X_train, y_train)

run_log["accuracy"] = model.score(X_test, y_test)
run_log["train_size"] = len(X_train)
run_log["test_size"] = len(X_test)
print("Result:", run_log)
```

---

## 2. Audit Everything

### Without guidelines

```python
import sqlite3

def update_participant_status(participant_id, new_status):
    conn = sqlite3.connect("trial.db")
    cursor = conn.cursor()
    cursor.execute(
        "UPDATE participants SET status = ? WHERE id = ?",
        (new_status, participant_id)
    )
    conn.commit()
    conn.close()
```

The record changes. There is no trace of what it was before, who changed it, when, or why.

### With guidelines

```python
import sqlite3
from datetime import datetime

def update_participant_status(participant_id, new_status, changed_by, reason):
    conn = sqlite3.connect("trial.db")
    cursor = conn.cursor()

    cursor.execute(
        "SELECT status FROM participants WHERE id = ?",
        (participant_id,)
    )
    row = cursor.fetchone()
    if row is None:
        raise ValueError(f"Participant {participant_id} not found.")
    previous_status = row[0]

    cursor.execute(
        "UPDATE participants SET status = ? WHERE id = ?",
        (new_status, participant_id)
    )

    cursor.execute(
        """
        INSERT INTO audit_log
            (participant_id, field, previous_value, new_value, changed_by, reason, timestamp)
        VALUES (?, ?, ?, ?, ?, ?, ?)
        """,
        (
            participant_id,
            "status",
            previous_status,
            new_status,
            changed_by,
            reason,
            datetime.utcnow().isoformat()
        )
    )

    conn.commit()
    conn.close()
```

---

## 3. Surgical Data Handling

### Without guidelines

```python
import pandas as pd

df = pd.read_csv("/Users/researcher/Desktop/patient_data_final.csv")

df = df.dropna()
df = df[df["age"] > 18]
df["bmi"] = df["weight"] / (df["height"] ** 2)

df.to_csv("/Users/researcher/Desktop/patient_data_clean.csv", index=False)
```

Path hardcoded to one machine. Rows dropped silently. Age filter applied without explanation. Units assumed.

### With guidelines

```python
import pandas as pd
import os
from datetime import datetime

INPUT_PATH = os.environ["CLINICAL_DATA_INPUT"]
OUTPUT_PATH = os.environ["CLINICAL_DATA_OUTPUT"]

df = pd.read_csv(INPUT_PATH)
initial_count = len(df)
print(f"Loaded {initial_count} records from {INPUT_PATH}")

# missing values in clinical data may indicate withdrawn consent or a data entry failure — log before discarding
missing_mask = df.isnull().any(axis=1)
missing_count = missing_mask.sum()
df = df[~missing_mask]
print(f"Dropped {missing_count} records with missing values ({missing_count/initial_count:.1%})")

# protocol section 3.2 requires participants to be 18 or older — records below this threshold are out of scope, not errors
age_mask = df["age"] < 18
excluded_age = age_mask.sum()
df = df[~age_mask]
print(f"Excluded {excluded_age} records: age < 18 (protocol inclusion criterion 3.2)")

# BMI — weight in kg, height in metres, per data dictionary v4
# If units are different, this calculation is wrong — verify before use
df["bmi"] = df["weight_kg"] / (df["height_m"] ** 2)
unexpected_bmi = df[(df["bmi"] < 10) | (df["bmi"] > 80)]
if len(unexpected_bmi) > 0:
    print(f"WARNING: {len(unexpected_bmi)} records with unexpected BMI — review before proceeding")

print({
    "timestamp": datetime.utcnow().isoformat(),
    "input": INPUT_PATH,
    "input_records": initial_count,
    "output_records": len(df),
    "dropped_missing": missing_count,
    "excluded_underage": excluded_age,
})

df.to_csv(OUTPUT_PATH, index=False)
```

---

## 4. Communicate Uncertainty

### Without guidelines

```python
from sklearn.linear_model import LogisticRegression

model = LogisticRegression()
model.fit(X_train, y_train)

prediction = model.predict(X_test)
print(f"Predicted outcome: {prediction[0]}")
```

A clinician sees a prediction with no confidence, no population context, no limitations.

### With guidelines

```python
import numpy as np
from sklearn.linear_model import LogisticRegression
from sklearn.utils import resample

RANDOM_SEED = 42

model = LogisticRegression()
model.fit(X_train, y_train)

predicted_proba = model.predict_proba(X_test)[0]
predicted_class = model.classes_[np.argmax(predicted_proba)]

# point estimate alone is not sufficient for clinical decision support — CI quantifies how much the estimate would shift on a different patient cohort
n_bootstraps = 1000
bootstrap_probas = []
for i in range(n_bootstraps):
    X_boot, y_boot = resample(X_train, y_train, random_state=RANDOM_SEED + i)
    m = LogisticRegression().fit(X_boot, y_boot)
    bootstrap_probas.append(m.predict_proba(X_test)[0][1])

ci_lower = np.percentile(bootstrap_probas, 2.5)
ci_upper = np.percentile(bootstrap_probas, 97.5)

print({
    "predicted_outcome": predicted_class,
    "probability": round(predicted_proba[1], 3),
    "95_ci": [round(ci_lower, 3), round(ci_upper, 3)],
    "training_population": "HFEA registry 2014-2023, UK fertility clinics only",
    "limitations": [
        "Trained on UK population — may not generalise to other healthcare systems",
        "Does not account for stimulation protocol variation across clinics",
        "Not validated on patients over 45"
    ]
})
```

