# Analisis Perilaku Pelanggan Jaringan Kios Offline

Analisis data pelanggan untuk memahami perilaku kunjungan, segmentasi pelanggan, faktor yang berkaitan dengan kunjungan ulang, serta pendekatan untuk memprioritaskan pelanggan yang berpotensi memberikan nilai tinggi.

## 📌 Tentang Project

Sebuah jaringan kios offline ingin memahami perilaku pelanggannya untuk menjawab tiga pertanyaan utama:

1. Bagaimana pelanggan dapat dikelompokkan berdasarkan perilaku dan nilai transaksi?
2. Faktor apa pada kunjungan pertama yang berkaitan dengan kemungkinan pelanggan melakukan kunjungan kembali?
3. Bagaimana perusahaan dapat memprioritaskan pelanggan yang memiliki potensi nilai tinggi?

Project ini menggunakan analisis eksploratif, segmentasi pelanggan, regresi logistik, Linear Discriminant Analysis (LDA), serta propensity scoring untuk menjawab pertanyaan tersebut.

## 📊 Dataset

Dataset terdiri dari dua sumber utama:

- `customers.csv` — informasi demografis pelanggan.
- `kiosk_visits.csv` — informasi transaksi dan kunjungan pelanggan.

Data mencakup **500 pelanggan, 1.116 kunjungan, dan 5 kota kios**.

## 🔎 Analisis yang Dilakukan

### 1. Segmentasi Pelanggan

Pelanggan dikelompokkan berdasarkan karakteristik perilaku dan nilai transaksi menggunakan **K-Means Clustering**.

Hasilnya menghasilkan empat segmen:

- **Loyal Regulars**
- **Promo-Driven Shoppers**
- **High-Value One-Timers**
- **Low-Value Browsers**

### 2. Analisis Kunjungan Ulang

Menganalisis hubungan karakteristik kunjungan pertama dengan kemungkinan pelanggan melakukan kunjungan kembali menggunakan:

- Logistic Regression
- Linear Discriminant Analysis (LDA)
- 5-fold Cross-Validation

### 3. Penargetan Pelanggan Bernilai Tinggi

Mendefinisikan pelanggan bernilai tinggi berdasarkan **future spend**, yaitu total pengeluaran setelah kunjungan pertama.

Model propensity kemudian digunakan untuk memberikan **prioritas relatif** terhadap pelanggan yang berpotensi masuk kelompok high-value.

## 💡 Key Findings

- **59,4% pelanggan melakukan kunjungan ulang**, menunjukkan bahwa repeat visit merupakan bagian penting dari basis pelanggan.
- **Loyal Regulars** hanya mencakup sekitar **27,6% pelanggan**, tetapi menghasilkan sekitar **49,4% revenue**.
- **Low-Value Browsers** mencakup sekitar **30,8% pelanggan**, tetapi hanya menghasilkan sekitar **7,5% revenue**.
- Model prediksi repeat visit menunjukkan **daya prediksi yang terbatas** berdasarkan karakteristik kunjungan pertama saja.
- Logistic Regression dan LDA menghasilkan hasil yang sangat serupa, sehingga Logistic Regression dipilih karena lebih mudah diinterpretasikan.
- Kelompok **Top 25% propensity** memiliki actual high-value rate sebesar **35,2%**, dibandingkan **33,0%** pada keseluruhan pelanggan.

## 🎯 Business Recommendations

Berdasarkan hasil analisis:

1. Memprioritaskan **Loyal Regulars** untuk strategi retensi dan loyalty.
2. Mengembangkan strategi untuk mengubah **High-Value One-Timers** menjadi pelanggan yang melakukan kunjungan ulang.
3. Menggunakan propensity score sebagai **sinyal prioritas relatif**, bukan sebagai satu-satunya dasar keputusan targeting.
4. Menguji strategi cross-selling dan engagement melalui eksperimen yang terukur.
5. Mengumpulkan fitur tambahan seperti recency, jarak ke kios, interaksi layanan, dan riwayat engagement untuk meningkatkan kemampuan prediksi model.

## 🛠️ Tools & Methods

**Tools:**
- Python
- Pandas
- NumPy
- Matplotlib
- Seaborn
- Scikit-learn
- Statsmodels

**Methods:**
- Exploratory Data Analysis
- K-Means Clustering
- Logistic Regression
- Linear Discriminant Analysis
- Cross-Validation
- Propensity Scoring
