# 🧠 PredectMyPolicy  
*An interactive Streamlit app to predict insurance policy renewal probability.*

---

## 🚀 Overview

**PredectMyPolicy** uses a trained **Serial Ensemble Model** (XGBoost → SVM → Logistic Regression) to estimate whether a policy will be **renewed** or **not renewed** based on customer and premium characteristics.  

The app allows users to:
- Upload a CSV file containing new policyholder records  
- Automatically clean, validate, and scale the data using the **same scaler** used during training  
- Run the pre-trained model to predict renewal outcomes  
- Download results with predicted labels and probabilities  

---

## 🧩 Model Bundle Details

The app loads the model artifacts from the directory:  
`model_bundle_serial_smote_v1/`

This bundle contains:
| File | Description |
|-------|--------------|
| `meta.json` | Metadata including stage order, thresholds, feature names, and label map |
| `scaler.joblib` | StandardScaler (fitted on training data) |
| `stage_1_xgb.joblib` | Stage-1 model (XGBoost) |
| `stage_2_svc.joblib` | Stage-2 model (Calibrated Linear SVC) |
| `stage_3_logreg.joblib` | Stage-3 model (Logistic Regression) |

**Training label convention:**
> `1 = non_renew`, `0 = renew`

---

## 📊 Input Format (CSV Upload)

Your uploaded CSV must contain the **exact same column names** as used during training:

| Feature | Description |
|----------|--------------|
| `perc_premium_paid_by_cash_credit` | % of premium paid by cash/credit |
| `Income` | Annual income of customer |
| `Count_3-6_months_late` | Payments delayed between 3–6 months |
| `Count_6-12_months_late` | Payments delayed between 6–12 months |
| `Count_more_than_12_months_late` | Payments delayed >12 months |
| `application_underwriting_score` | Risk/underwriting score |
| `no_of_premiums_paid` | Number of premiums paid so far |
| `premium` | Premium amount |
| `age_in_years` | Age of customer |

> 🧹 The app will automatically drop any unknown or categorical fields.

---

## 🖥️ How to Use (Streamlit Cloud)

1. Go to your deployed app link (e.g., [https://predectmypolicy.streamlit.app](https://predectmypolicy.streamlit.app))  
2. Upload your `.csv` file with the required columns  
3. Wait for processing — predictions will appear in a results table  
4. Download predictions as a `.csv` for further analysis  

---

## 🧮 Output

| Column | Meaning |
|---------|----------|
| `Predicted_Label` | `renew` or `non_renew` |
| `Prediction_Prob` | Model-derived pseudo probability of non-renewal |
| (Optional) All stage probabilities | If debug mode is enabled |

Example:

| Policy_ID | Predicted_Label | Prediction_Prob |
|------------|-----------------|-----------------|
| 1001 | renew | 0.18 |
| 1002 | non_renew | 0.76 |

---

## 🛠️ Local Setup (for development)

```bash
# 1. Clone the repo
git clone https://github.com/<yourusername>/PredectMyPolicy.git
cd PredectMyPolicy

# 2. Create a virtual environment
python -m venv .venv
source .venv/bin/activate  # On Mac/Linux
.venv\Scripts\activate     # On Windows

# 3. Install dependencies
pip install -r requirements.txt

# 4. Run Streamlit app
streamlit run app.py
