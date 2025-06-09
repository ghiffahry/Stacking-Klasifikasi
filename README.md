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


