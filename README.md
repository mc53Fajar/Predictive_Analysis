# Laporan Proyek Machine Learning - Fajar Rahmat

## Domain Proyek

Dalam beberapa tahun terakhir, tren kerja fleksibel seperti *remote working* dan *hybrid* mengalami pertumbuhan pesat, terutama dipicu oleh pandemi COVID-19. Model kerja seperti ini telah mendisrupsi banyak aspek dunia kerja, salah satunya adalah struktur penggajian. Ketika karyawan tidak lagi terikat pada lokasi fisik tertentu, muncul pertanyaan mengenai bagaimana menentukan gaji yang adil dan kompetitif.

Proyek ini dirancang untuk mengkaji faktor-faktor apa saja yang mempengaruhi gaji karyawan dalam skenario kerja fleksibel tersebut. Dengan membangun model prediksi berbasis machine learning, perusahaan diharapkan bisa menyusun struktur penggajian yang adil dan pekerja dapat memperoleh gambaran realistis tentang ekspektasi gaji mereka.

## Business Understanding

### Problem Statements
- Bagaimana cara memprediksi gaji tahunan pekerja berbasis atribut pekerjaan dan latar belakang mereka?
- Apakah fitur-fitur seperti jabatan, lokasi, pengalaman, dan jenis pekerjaan berpengaruh signifikan terhadap prediksi gaji?

### Goals
- Mengembangkan model prediksi yang dapat mengestimasi gaji tahunan dalam format USD.
- Memberikan interpretasi terhadap hubungan antara fitur-fitur pekerjaan dan gaji.
- Menghasilkan evaluasi performa dari dua model berbeda.

### Solution Statements
- Mengimplementasikan dua algoritma regresi: **Random Forest Regressor** dan **XGBoost Regressor**.
- Mengevaluasi performa model dengan metrik regresi: MAE, MSE, RMSE, dan R².
- Menentukan model terbaik berdasarkan hasil evaluasi metrik dan interpretabilitas.

## Data Understanding

Dataset yang digunakan berasal dari Kaggle: [Work From Anywhere Salary Insight 2024](https://www.kaggle.com/datasets/atharvasoundankar/work-from-anywhere-salary-insight-2024)

Dataset ini terdiri dari 500 data observasi yang mencakup:
- **Company**: Nama perusahaan.
- **Job Title**: Jabatan yang dipegang.
- **Industry**: Sektor industri tempat bekerja.
- **Location**: Lokasi geografis kerja.
- **Employment Type**: Jenis kerja (Full-time, Part-time, dll).
- **Experience Level**: Level senioritas.
- **Remote Flexibility**: Mode kerja (Remote, Hybrid, Onsite).
- **Salary (Annual)**: Gaji tahunan.
- **Currency**: Mata uang gaji.
- **Years of Experience**: Lama pengalaman profesional.
- **Job Satisfaction Score**: Skor kepuasan kerja.
- **Last Promotion (Years Ago)**: Tahun sejak terakhir kali dipromosikan.

## Data Preparation

Langkah-langkah preprocessing meliputi:
- **Konversi Mata Uang**: Seluruh nilai gaji dikonversi ke USD menggunakan nilai tukar tetap, lalu kolom `Salary (Annual)` dan `Currency` dihapus.
- **Encoding Kategorikal**: Label encoding digunakan untuk mengubah data kategorikal menjadi numerik.
- **Pembuangan Kolom**: Kolom `Tech Stack` dan `Perks` dihapus karena tidak relevan.
- **Pembersihan Outlier**: Outlier diperiksa melalui metode IQR dan visualisasi boxplot.
- **Split Data**: Dataset dibagi menjadi 70% data pelatihan dan 30% data pengujian.

## Modeling

### Algoritma 1: Random Forest Regressor
- **Parameter utama**: `n_estimators=100`, `random_state=42`
- Cocok digunakan untuk dataset tabular dan robust terhadap noise.
- Tidak sensitif terhadap label encoding pada fitur kategorikal.

### Algoritma 2: XGBoost Regressor
- **Parameter utama**: `n_estimators=100`, `learning_rate=0.1`, `max_depth=3`, `random_state=42`
- Unggul dalam hal efisiensi dan akurasi.
- Cenderung sensitif terhadap fitur kategorikal dengan banyak kelas jika tidak ditangani dengan baik.

Model dilatih menggunakan dataset yang telah dibersihkan dan distandardisasi. Proses pelatihan dilakukan tanpa tuning lanjutan untuk membandingkan baseline dari kedua model.

## Evaluation

### Metrik Evaluasi yang Digunakan:
- **MAE (Mean Absolute Error)**: Rata-rata selisih absolut antara nilai prediksi dan aktual.
- **MSE (Mean Squared Error)**: Rata-rata kuadrat dari selisih prediksi dan aktual.
- **RMSE (Root Mean Squared Error)**: Akar dari MSE, digunakan karena berada dalam skala yang sama dengan target.
- **R² Score**: Mengukur proporsi variansi yang dapat dijelaskan oleh model.

| Model              | MAE     | RMSE     | R² Score |
|--------------------|---------|----------|----------|
| Random Forest      | 61,872  | 74,719   | -0.129   |
| XGBoost Regressor  | 67,888  | 83,568   | -0.412   |

### Interpretasi
- **Random Forest** memiliki performa lebih baik dari XGBoost, meskipun keduanya belum cukup baik.
- R² Score yang negatif menandakan bahwa model belum bisa menjelaskan variasi gaji secara efektif.
- Performa buruk kemungkinan disebabkan oleh:
  - Kompleksitas data kategorikal.
  - Minimnya feature engineering.
  - Kurangnya tuning model.

### Rekomendasi
- Terapkan **feature selection/engineering** lanjutan.
- Gunakan **one-hot encoding** atau metode encoding lain yang lebih sesuai.
- Lakukan **hyperparameter tuning** menggunakan GridSearch atau RandomizedSearch.
- Coba model alternatif seperti **CatBoost** atau **LightGBM** untuk membandingkan hasil.

---

