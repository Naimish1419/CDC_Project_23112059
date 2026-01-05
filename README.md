# 🏡 Multimodal Real Estate Price Prediction using Tabular Data and Satellite Imagery

## 📌 Project Overview
This project builds a **multimodal machine learning system** to predict residential property prices by combining **structured tabular housing data** with **satellite imagery–based visual context**.

While tabular features such as size, quality, and location explain most of the price variation, they often miss **neighborhood-level environmental signals** (e.g., greenery, urban density, spatial layout).  
To address this, the project integrates **CNN-based satellite image embeddings** using a **residual learning framework**, allowing images to refine price predictions beyond structured data.

---

## 🎯 Objectives
- Develop a strong **tabular baseline model** for price prediction  
- Acquire **satellite images programmatically** using geographic coordinates  
- Perform **EDA, spatial analysis, and visual-financial insights**  
- Extract **image embeddings using ResNet50**  
- Combine tabular and image data via **residual learning**  
- Compare **Tabular-only vs Tabular + Satellite Image** performance  
- Ensure interpretability using **Grad-CAM visualizations**

---

## 📊 Dataset Description

### 🔹 Tabular Data
The structured dataset includes:
- **Structural features**: bedrooms, bathrooms, floors, basement
- **Size & quality**: sqft_living, grade, condition, view, waterfront
- **Temporal features**: year_sold, month_sold, house_age
- **Spatial features**: latitude, longitude, distance from city center
- **Engineered ratios**: space efficiency and layout indicators

### 🔹 Satellite Images
- Images fetched using **latitude & longitude**
- Source: **ESRI World Imagery Tile Service**
- Captures neighborhood context such as:
  - Green cover
  - Road networks
  - Built-up density
  - Spatial organization

---

## 🛠 Feature Engineering Summary
After detailed EDA and spatial analysis, the following transformations were applied:

- Removed non-informative identifiers (`id`, `zipcode`)
- Extracted transaction year and month
- Converted renovation year into a binary flag
- Computed property age at sale
- Applied **log transformation** to price and living area
- Created **space-efficiency ratios** (sqft per bedroom, bath per bedroom)
- Added **structural ratios** (above-ground proportion)
- Engineered **distance from city center**
- Introduced geographic interaction terms
- Removed redundant raw features to reduce multicollinearity

---

## 🔍 Exploratory Data Analysis & Visual Insights
EDA and visualization were used to identify key price drivers:

- Price distribution across segments
- Feature-wise price impact (floors, basement, view, renovation)
- Spatial clustering and geofencing
- Market segmentation using geographic proximity
- Comparison of **high-value vs low-value neighborhoods** via satellite images
- Analysis of built-up density and greenery patterns

These insights directly guided feature engineering and model design.

---

## 🧠 Modeling Approach

### 1️⃣ Tabular Baseline Models
The following models were evaluated using tabular data:
- Linear Regression
- Neural Network
- **XGBoost Regressor**

**XGBoost** achieved the strongest performance and was selected as the base model.

---

### 2️⃣ Image Feature Extraction
- Satellite images processed using **pre-trained ResNet50**
- CNN used as a **frozen feature extractor**
- Generated **2048-dimensional embeddings**
- Reduced to **512 dimensions using PCA**

---

### 3️⃣ Multimodal Residual Learning
Instead of predicting price directly from images, a **residual learning strategy** was used:

1. XGBoost predicts base price from tabular data (`ŷ_tabular`)
2. Residual computed as:  
   `residual = y_true − ŷ_tabular`
3. Image embeddings used to predict residual via XGBoost
4. Final price computed as:  
   `final_price = ŷ_tabular + residual_pred`

This allows images to model **neighborhood-level price corrections**.

---

## 🧪 Experimental Setup
- Data split: **70% Train / 15% Validation / 15% Test**
- Tabular features scaled using **StandardScaler**
- PCA applied only to image embeddings
- XGBoost used for both base and residual models
- Fixed random seed for reproducibility

---

## 📈 Results: Tabular vs Multimodal

| Model | Data Used | R² (Test) | RMSE (Test) |
|-----|---------|-----------|-------------|
| Linear Regression | Tabular | 0.7855 | 0.2465 |
| Neural Network | Tabular | 0.8867 | 0.1791 |
| XGBoost | Tabular | 0.9084 | 0.1611 |
| **XGBoost (Multimodal Residual)** | **Tabular + Images** | **0.9096** | **0.1600** |

### Key Insight
Tabular data already explains **~90% of price variance**.  
Satellite imagery provides a **small but consistent and meaningful improvement**, capturing environmental and neighborhood-level effects.

---

## 🔍 Model Explainability (Grad-CAM)
Grad-CAM visualizations reveal that the CNN focuses on:
- Green spaces
- Road connectivity
- Neighborhood layout

This confirms the model learns **economically meaningful visual cues** rather than noise.

---

## ⚙️ Setup & Execution Instructions

Follow the steps below to set up the environment and reproduce the results.

### 1. Create a Virtual Environment (Recommended)

bash
- python -m venv venv
- source venv/bin/activate        # macOS / Linux
- venv\Scripts\activate           # Windows

### 2. Install Dependencies
- pip install numpy pandas scikit-learn xgboost torch torchvision pillow tqdm requests folium matplotlib seaborn

### 3. Prepare the Dataset
- Place train.xlsx in the project root directory
- Ensure the dataset contains latitude (lat) and longitude (long) columns (required for satellite imagery)

### 4. Run Exploratory Data Analysis (EDA)
- jupyter notebook preprocessing.ipynb

### 5. Download Satellite Images & Generate CNN Embeddings
- python data_fetcher.py

This step:
- Downloads satellite images using ESRI World Imagery
- Extracts image embeddings using ResNet50
- ⚠️ This step may take time depending on network speed.

### 7. Apply Pipeline to Test Data
  
- Replace the training dataset with the test dataset
- Run the same preprocessing and embedding pipeline
- Use trained models for prediction
- Do NOT refit scalers, PCA, or CNN models to avoid data leakage

### 🔁 Reproducibility Notes
- Fixed random seeds are used
- CNN weights are frozen (no fine-tuning)
- Large generated files (satellite images and embeddings) are excluded and can be regenerated

### 6. Train Models & Evaluate Performance
- jupyter notebook Model_Training.ipynb

This trains:
- Tabular models (Linear Regression, Neural Network, XGBoost)
- Multimodal residual model using image embeddings
- Evaluates performance using R² and RMSE

---



## 📂 Project Structure
RealEstate-Multimodal/
│
├── preprocessing.ipynb
├── modeling.ipynb
├── data_fetcher.py
├── README.md
├──



---

## 🔮 Future Work
- Higher-resolution and multi-scale satellite imagery
- Temporal satellite data
- End-to-end multimodal deep learning
- Integration of POI and socioeconomic datasets

---

## 👤 Author
**Name:** Naimish Mehta  
**Enrollment No.:** 23112059  

---

## 📎 References
- ESRI World Imagery Tile Service  
- ResNet50 (ImageNet pre-trained model)  
- XGBoost Documentation  
- Scikit-learn Library  

---

## 📎 License
This project is intended for **academic and educational purposes only**.

