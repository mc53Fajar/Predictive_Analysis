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
Dataset yang digunakan dalam proyek ini berjudul “Work From Anywhere Salary Data”, yang terdiri dari 500 data terstruktur mengenai individu yang bekerja secara remote, hybrid, maupun onsite. Data ini mencakup berbagai jabatan, industri, tingkat pengalaman, lokasi kerja, serta informasi mengenai penggajian. Dataset ini bersifat sintetis namun merepresentasikan pola dan tren yang lazim di dunia kerja global saat ini

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
- **Tech Stack**: Teknologi yang digunakan(Misalnya: Python, JavaScript, SQL,dsb)
- **Perks** : Tunjangan tambahan yang diberikan (misalnya: asuransi, WFH allowance)
- **Last Promotion (Years Ago)**: Tahun sejak terakhir kali dipromosikan.

## Data Preparation
### Memeriksa Missing Value
tidak terdapat missing value pada dataset ini. Pemeriksaan missing value ini penting untuk menjaga kualitas data.

### Memeriksa Data Duplikat
Tidak ada data duplikat pada dataset ini. Pemeriksaan data duplikat dilakukan untuk menjaga kualitas data, agar tidak terjadi bias dalam analisis.

### Tranformasi
Langkah-langkah preprocessing meliputi:
- **Konversi Mata Uang**: Seluruh nilai gaji dikonversi ke USD menggunakan nilai tukar tetap, lalu kolom `Salary (Annual)` dan `Currency` dihapus.
- **Encoding Kategorikal**: Label encoding digunakan untuk mengubah data kategorikal menjadi numerik.
- **Pembuangan Kolom**: Kolom `Tech Stack` dan `Perks` dihapus karena tidak relevan.

### Memeriksa Data Outlier
Setelah ditransformasi data, tidak ditemukan adanya outlier pada dataset. Hal ini dikonfirmasi melalui deteksi IQR dan visualisasi boxplot untuk fitur-fitur numerik yang relevan.

### Data Cleaning
Tidak ada proses pembersihan data yang dilakukan karena dataset sudah terbukti bersih dari missing value, duplikat, dan outlier.

### Data Splitting
Data dibagi menjadi fitur (X) dan target (y). Fitur X mencakup semua kolom kecuali Salary (USD), sedangkan y adalah kolom Salary (USD). Dataset kemudian dibagi menjadi data training dan testing dengan proporsi 70:30 (70% untuk training, 30% untuk testing).
- Jumlah data total: 500
- Jumlah data training: 350
- Jumlah data testing: 150

## Exploratory Data Analysis (EDA)
### Univariate Analysis
- Secara keseluruhan, distribusi fitur pada data ini cukup merata di tiap kolom.
- Years of Experience: Distribusi pengalaman tersebar cukup merata dari 0 hingga sekitar 15 tahun, namun sedikit lebih rendah pada tahun-tahun awal (0-2 tahun) dan sekitar 5-7 tahun.
- Job Satisfaction Score (1-10): Skor kepuasan kerja relatif merata di seluruh rentang 1 hingga 10, menunjukkan beragam tingkat kepuasan tanpa satu skor yang mendominasi.
- Last Promotion (Years Ago): Mayoritas promosi terjadi dalam 0-1 tahun terakhir, menunjukkan pola promosi yang relatif sering atau baru saja terjadi di antara para pekerja dalam dataset.
- Salary (USD): Mayoritas gaji terkonsentrasi pada rentang yang lebih rendah (di bawah $50.000), dengan distribusi yang sangat miring ke kanan. Hal ini berbeda dari visualisasi awal Salary (Annual) dan dimungkinkan setelah disamakannya mata uang yang digunakan serta dipengaruhi konversi dari AUD dan INR yang cukup rendah, mengingat pekerja yang menggunakan mata uang tersebut cukup banyak.
- Variabel Kategorikal/Diskrit (Company, Job Title, Industry, Location, Employment Type, Experience Level): Menunjukkan distribusi yang relatif seragam atau merata di antara kategori-kategori mereka.

### Multivariative Analysis (EDA)
- Gaji vs Perusahaan (Company): Tidak ada korelasi linear yang jelas atau pola signifikan yang terlihat. Gaji menunjukkan variasi yang sangat luas (dari rendah hingga tinggi) di setiap kategori perusahaan. Ini mengindikasikan bahwa gaji tidak ditentukan secara langsung oleh perusahaan, dan ada rentang gaji yang lebar dalam setiap perusahaan.
- Gaji vs Posisi Pekerjaan (Job Title): Mirip dengan perusahaan, tidak ada korelasi linear yang jelas. Gaji sangat bervariasi di setiap kategori posisi pekerjaan. Ini menunjukkan bahwa gaji tidak secara langsung tergantung pada jenis posisi pekerjaan yang spesifik dalam konteks linear.
- Gaji vs Industri (Industry): Tidak ada pola linear atau korelasi yang menonjol. Setiap kategori industri menunjukkan rentang gaji yang luas dari nilai rendah hingga tinggi. Ini berarti industri saja tidak secara langsung menunjukkan hubungan linear yang kuat dengan tingkat gaji.
- Gaji vs Tipe Pekerjaan (Employment Type): Tidak ada korelasi linear yang jelas. Gaji memiliki variasi yang luas di setiap tipe pekerjaan. Ini mengindikasikan bahwa tipe pekerjaan tertentu tidak secara langsung berkorelasi secara linear dengan tingkat gaji.
- Gaji vs Pengalaman Kerja (Tahun): Tidak ada korelasi linear positif yang kuat. Meskipun ada individu dengan pengalaman tinggi yang memiliki gaji tinggi, ada juga individu dengan pengalaman rendah yang memiliki gaji tinggi, dan sebaliknya. Titik-titik tersebar cukup acak, menunjukkan bahwa pengalaman kerja saja tidak selalu berkorelasi secara linear dengan gaji yang lebih tinggi.
- Gaji vs Skor Kepuasan Kerja: Tidak ada pola linear atau korelasi yang jelas. Gaji menunjukkan rentang yang sangat luas untuk setiap skor kepuasan kerja. Artinya, gaji tidak berkorelasi secara linear dengan tingkat kepuasan kerja.
- Gaji vs Terakhir Promosi (Tahun Lalu): Tidak ada korelasi linear yang jelas. Gaji bervariasi secara luas terlepas dari berapa tahun yang lalu seseorang terakhir dipromosikan. Seseorang yang baru dipromosikan bisa memiliki gaji rendah atau tinggi, sama seperti yang sudah lama tidak dipromosikan.
- Kesimpulan Umum Multivariat: Sebagian besar fitur individual yang diplot terhadap gaji (Perusahaan, Posisi Pekerjaan, Industri, Tipe Pekerjaan, Skor Kepuasan Kerja, Pengalaman Kerja, Terakhir Promosi) menunjukkan variasi gaji yang sangat luas dalam setiap kategorinya dan tidak ada korelasi linear yang kuat. Ini menyiratkan bahwa gaji kemungkinan besar dipengaruhi oleh kombinasi banyak faktor yang kompleks, dan tidak ada satu fitur pun dari yang ditampilkan yang secara tunggal memiliki hubungan linear langsung yang dominan dengan gaji.

## Modeling
Untuk menyelesaikan permasalahan prediksi gaji tahunan (Salary (USD)), dua algoritma regresi dipilih: Random Forest Regressor dan XGBoost Regressor. Keduanya dipilih karena mampu menangani data dengan banyak variabel kategorikal (terutama yang telah diubah menjadi numerik menggunakan LabelEncoder) dan non-linear relationship antara fitur dan target.

### Algoritma 1: Random Forest Regressor
Random Forest cocok digunakan karena dataset ini memiliki banyak fitur kategorikal yang sudah diubah ke bentuk numerik menggunakan Label Encoding. Model ini tidak sensitif terhadap urutan angka hasil encoding, sehingga tetap dapat menghasilkan prediksi yang baik. Selain itu, jumlah kategori pada setiap fitur tidak terlalu banyak (umumnya <10), sehingga kompleksitas model tetap terjaga.

**Cara Kerja:**
Random Forest merupakan algoritma ensemble berbasis decision tree. Ia membangun banyak pohon keputusan (tree) saat proses pelatihan dan menggabungkan prediksi dari semua pohon tersebut untuk menghasilkan output akhir. Dalam regresi, hasil akhirnya adalah rata-rata dari semua prediksi pohon.

**Keunggulan:**
- Mengurangi risiko overfitting dibandingkan decision tree tunggal
- Cocok untuk data dengan fitur kompleks dan non-linear

**Parameter yang Digunakan:**
- n_estimators=100: Jumlah pohon yang dibangun dalam forest.
- random_state=42: Menjaga konsistensi hasil (reproducibility).
- max_depth=None: Kedalaman pohon tidak dibatasi, artinya setiap pohon bisa tumbuh bebas sampai daun.

### Algoritma 2: XGBoost Regressor
XGBoost juga sesuai karena mampu menangani data kategorikal yang sudah di-label encoding dengan baik. Model ini kuat terhadap data tabular seperti yang digunakan dalam proyek ini, dan memiliki kemampuan generalisasi yang baik, terutama jika dilakukan tuning hyperparameter. Jumlah kategori yang relatif sedikit juga membuat proses training lebih efisien.

**Cara Kerja:**
XGBoost adalah algoritma boosting berbasis pohon keputusan yang bekerja dengan cara membangun model secara berurutan, di mana setiap model baru memperbaiki kesalahan model sebelumnya menggunakan gradient descent. Proses ini dioptimalkan secara efisien, menjadikannya sangat cepat dan akurat.

**Keunggulan:**
- Performa tinggi (baik akurasi maupun kecepatan)
- Dilengkapi dengan regularisasi untuk mencegah overfitting
- Sangat baik dalam menangani missing value dan fitur kategorikal

**Parameter yang Digunakan:**
- n_estimators=100: Jumlah boosting rounds (jumlah pohon).
- learning_rate=0.1: Seberapa besar kontribusi tiap pohon terhadap model akhir.
- max_depth=3: Batas kedalaman tiap pohon untuk mencegah overfitting.
- random_state=42: Untuk menjaga konsistensi hasil.

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

**Hasil Proyek berdasarkan metrik evaluasi:**
1. Random Forest lebih unggul dari XGBoost dalam memprediksi gaji (Salary (USD)), ditunjukkan dari nilai:

- MAE lebih kecil: $61.872 (Random Forest) vs $67.888 (XGBoost). Ini berarti rata-rata kesalahan prediksi Random Forest lebih kecil sekitar $6.000.
- R² Score lebih tinggi: -0.129 (Random Forest) vs -0.412 (XGBoost). Meskipun keduanya masih memiliki skor negatif (artinya performanya lebih buruk dari rata-rata), Random Forest masih lebih "dekat" menuju model yang baik.

2. RMSE sama pada kedua model (~ $83.568), menunjukkan bahwa keduanya menghasilkan penyebaran error yang cukup besar terhadap nilai aktual.

3. Nilai R² negatif pada kedua model mengindikasikan bahwa model belum mampu menjelaskan variansi dari target Salary (USD) dengan baik. Ini bisa disebabkan oleh beberapa faktor, seperti dataset yang terlalu kecil, banyaknya fitur kategorikal yang belum cukup diolah, data mungkin memiliki multikolinearitas atau noise tinggi, atau skala antar fitur yang berbeda memengaruhi performa model.

**Insight:**

Random Forest lebih cocok untuk dataset ini dibandingkan XGBoost, walaupun keduanya masih perlu ditingkatkan performanya. Peningkatan bisa dilakukan melalui feature selection, feature engineering, standardisasi/normalisasi fitur numerik, atau hyperparameter tuning pada model.

---

