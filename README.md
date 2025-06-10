# Stacking-Klasifikasi

## Deskripsi Proyek  
Portofolio ini menampilkan studi komprehensif atas **pemodelan klasifikasi** tingkat kemiskinan makanan rumah tangga menggunakan data makroekonomi Provinsi Jawa Barat. Pendekatan yang diadopsi mencakup dua keluarga metode utama—Support Vector Machine (SVM) sebagai algoritma berbasis jarak dan Random Forest (RF) sebagai algoritma ensemble pohon—serta teknik **stacking** hibrid RF-SVM yang memanfaatkan XGBoost sebagai final estimator. Tujuan utama adalah mengevaluasi ketangguhan model terhadap pencilan dan variasi kondisi simulasi melalui perbandingan kinerja model tunggal dan ensemble.

## Struktur Repository   
- `src/`  
  Modul Python untuk pra-pemrosesan, penerapan SVM, Random Forest, dan pipeline stacking (RF + SVM → XGBoost).  
- `notebooks/`  
  Jupyter Notebook analisis EDA, seleksi fitur RFE, penanganan ketidakseimbangan dengan ADASYN, serta visualisasi hasil.  
- `reports/`  
  Ringkasan grafik dan tabel perbandingan metrik (balanced accuracy, sensitivity, specificity) di berbagai skenario simulasi.  
- `requirements.txt`  
  Dependensi Python (scikit-learn, xgboost, imbalanced-learn, pandas, matplotlib, dll).

## Metodologi  

### 1. Pra-Pemrosesan Data  
Data SUSENAS BPS 2023 dikombinasikan dengan variabel makroekonomi Provinsi Jawa Barat. Tahapan meliputi pembersihan nilai null/outlier, normalisasi skala (StandardScaler), dan reduksi skewness menggunakan transformasi Box-Cox. Pencilan ekstrem diidentifikasi melalui Z-score (threshold ±2.5) dan dihapus untuk menjaga integritas model.

### 2. Penanganan Ketidakseimbangan  
Metode **ADASYN** (Adaptive Synthetic Sampling) diterapkan pada data latih untuk menyeimbangkan proporsi kelas minoritas (miskin makanan) dan mayoritas, sehingga model dapat mempelajari pola representatif dari kedua kelas.

### 3. Seleksi Fitur  
**Recursive Feature Elimination (RFE)** dengan estimator dasar (RF atau SVM) digunakan untuk memilih subset fitur paling informatif—termasuk konsumsi kalori, pengeluaran bahan pangan, dan indikator makro—untuk meningkatkan efisiensi dan interpretabilitas model.

### 4. Pengembangan Model  
- **Random Forest (RF)**  
  Melatih model dengan tuning `n_estimators`, `max_depth`, dan `max_features` melalui grid search, dilanjutkan stratified K-fold CV (k=10).  
- **Support Vector Machine (SVM)**  
  Menggunakan kernel RBF, parameter `C` dan `gamma` dioptimalkan via grid search, dengan stratified K-fold CV (k=10).  
- **Stacking RF-SVM + XGBoost**  
  Prediksi probabilistik dari RF dan SVM digabungkan sebagai fitur input ke XGBoost sebagai final estimator. Hyperparameter XGBoost (`eta`, `max_depth`, `subsample`) dioptimalkan, juga diintegrasikan ke dalam pipeline stacking dengan stratified K-fold CV.

### 5. Evaluasi dan Simulasi Kondisi  
Model dievaluasi menggunakan metrik **balanced accuracy**, **sensitivity**, dan **specificity** pada data uji. Simulasi kondisi meliputi fluktuasi inflasi tinggi, guncangan nilai tukar, dan perubahan PDB per kapita untuk mengukur robustitas model terhadap variasi makroekonomi.

## Hasil & Temuan Utama  
- **RF** menunjukkan stabilitas tinggi terhadap pencilan, namun memiliki sensitivity lebih rendah dalam mendeteksi kelas miskin makanan.  
- **SVM** unggul dalam sensitivity berkat kemampuannya memodelkan batas keputusan non-linear, namun rentan terhadap pencilan.  
- **Stacking RF-SVM + XGBoost** mencapai kinerja terbaik secara keseluruhan dengan balanced accuracy **0.81**, sensitivity **0.75**, dan specificity **0.88**, memadukan keunggulan kedua base learners sekaligus meningkatkan robustitas melalui boosting.

## Deskripsi Proyek  
This portfolio presents a comprehensive study on the **classification modeling** of household food poverty levels using macroeconomic data from West Java Province. The adopted approach involves two major families of methods—Support Vector Machine (SVM) as a distance-based algorithm and Random Forest (RF) as a tree-based ensemble algorithm—along with a hybrid **stacking** technique that integrates RF and SVM predictions using XGBoost as the final estimator. The primary objective is to evaluate the robustness of the models against outliers and varying simulation conditions by comparing the performance of individual models and the ensemble approach.

## Repository Structure   
- `src/`  
  Python modules for preprocessing, implementing SVM, Random Forest, and the stacking pipeline (RF + SVM → XGBoost).  
- `notebooks/`  
  Jupyter Notebooks for EDA analysis, RFE feature selection, handling imbalance using ADASYN, and result visualization.  
- `reports/`  
  Summary of graphs and tables comparing metrics (balanced accuracy, sensitivity, specificity) across simulation scenarios. 
- `requirements.txt`  
  Python dependencies (scikit-learn, xgboost, imbalanced-learn, pandas, matplotlib, etc).

## Methodology

### 1. Data Preprocessing 
The 2023 SUSENAS BPS data was combined with macroeconomic variables of West Java Province. The stages include handling missing/outlier values, scale normalization (StandardScaler), and skewness reduction using Box-Cox transformation. Extreme outliers were identified using Z-score (threshold ±2.5) and removed to maintain model integrity.

### 2. Data Imbalance Handling 
**ADASYN** (Adaptive Synthetic Sampling) was applied to the training data to balance the proportion of the minority class (food-poor households) and the majority class, enabling the model to learn representative patterns from both classes.

### 3. Feature Selection  
**Recursive Feature Elimination (RFE)** with base estimators (RF or SVM) was used to select the most informative subset of features—including calorie consumption, food expenditure, and macroeconomic indicators—to improve model efficiency and interpretability.

### 4. Pengembangan Model  
- **Random Forest (RF)**  
  Trained using grid search for `n_estimators`, `max_depth`, and `max_features` followed by stratified K-fold CV (k=10).  
- **Support Vector Machine (SVM)**  
  Used the RBF kernel, with `C` and `gamma` optimized via grid search and stratified K-fold CV (k=10).  
- **Stacking RF-SVM + XGBoost**  
  Probabilistic predictions from RF and SVM were combined as input features to XGBoost as the final estimator. XGBoost hyperparameters (`eta`, `max_depth`, `subsample`) were optimized and integrated into the stacking pipeline using stratified K-fold CV.

### 5. Evaluation and Scenario Simulation 
The models were evaluated using **balanced accuracy**, **sensitivity**, and **specificity** on the test data. Scenario simulations included high inflation fluctuations, exchange rate shocks, and changes in GDP per capita to assess model robustness against macroeconomic variability.

## Hasil & Temuan Utama  
- **RF** showed high stability against outliers but had lower sensitivity in detecting food-poor households.
- **SVM** excelled in sensitivity due to its ability to model non-linear decision boundaries, but it was vulnerable to outliers.  
- **Stacking RF-SVM + XGBoost** achieved the best overall performance with balanced accuracy **0.81**, sensitivity **0.75**, and specificity **0.88**, combining the strengths of both base learners while enhancing robustness through boosting.
