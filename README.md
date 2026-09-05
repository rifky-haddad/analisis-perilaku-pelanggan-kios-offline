# Analisis Perilaku Pelanggan Jaringan Kios Offline

Analisis data pelanggan untuk memahami perilaku kunjungan, segmentasi pelanggan, faktor yang berkaitan dengan kunjungan ulang, serta pendekatan untuk memprioritaskan pelanggan yang berpotensi memberikan nilai tinggi.

📊 **500 Pelanggan** · 🛍️ **1.116 Kunjungan** · 🎯 **4 Segmen Pelanggan** · 🔁 **59,4% Repeat Visitor**

---

## 📌 Tentang Project

Sebuah jaringan kios offline ingin memahami perilaku pelanggannya untuk menjawab tiga pertanyaan utama:

1. Bagaimana pelanggan dapat dikelompokkan berdasarkan perilaku dan nilai transaksi?
2. Faktor apa pada kunjungan pertama yang berkaitan dengan kemungkinan pelanggan melakukan kunjungan kembali?
3. Bagaimana perusahaan dapat memprioritaskan pelanggan yang memiliki potensi nilai tinggi?

Project ini menggunakan analisis eksploratif, segmentasi pelanggan, regresi logistik, Linear Discriminant Analysis (LDA), evaluasi cross-validation, dan propensity scoring.

---

## 🎯 Tujuan Analisis

Analisis dilakukan untuk:

- Memahami pola kunjungan dan nilai transaksi pelanggan.
- Mengidentifikasi kelompok pelanggan dengan karakteristik perilaku yang berbeda.
- Menganalisis faktor pada kunjungan pertama yang berkaitan dengan kunjungan ulang.
- Membandingkan Logistic Regression dan LDA dalam memprediksi repeat visit.
- Mengidentifikasi pelanggan yang memiliki potensi future value lebih tinggi.
- Memberikan rekomendasi bisnis yang dapat digunakan untuk strategi retensi dan customer targeting.

---

## 📊 Dataset

Project menggunakan dua dataset:

### `customers.csv`

Berisi informasi tingkat pelanggan, antara lain:

- Customer ID
- Age Band
- Gender
- Total Visits
- Total Spend
- Average Basket Value

### `kiosk_visits.csv`

Berisi informasi setiap kunjungan pelanggan, antara lain:

- Customer ID
- Visit Date
- Kiosk ID
- City
- Visit Number
- Dwell Time
- Promo Exposure
- Spend Amount
- Items Purchased
- Payment Method

Dataset mencakup **500 pelanggan, 1.116 kunjungan, dan 5 kota kios**.

---

## 🔎 Metodologi Analisis

### 1. Data Quality & Customer-Level Aggregation

Tahap awal dilakukan untuk memahami struktur data, memeriksa kualitas data, serta memastikan konsistensi antara data pelanggan dan data kunjungan.

Selanjutnya, data kunjungan diagregasi ke tingkat pelanggan untuk membentuk fitur perilaku seperti:

- Total kunjungan
- Total pengeluaran
- Rata-rata nilai transaksi
- Jumlah item
- Paparan promo
- Durasi kunjungan

---

### 2. Segmentasi Pelanggan

Pelanggan dikelompokkan menggunakan **K-Means Clustering** berdasarkan karakteristik perilaku dan nilai transaksi.

Jumlah cluster dibandingkan menggunakan **Silhouette Score**, kemudian dipilih **4 segmen pelanggan** yang memiliki karakteristik bisnis yang berbeda:

- **Loyal Regulars**
- **Promo-Driven Shoppers**
- **High-Value One-Timers**
- **Low-Value Browsers**

Hasil segmentasi menunjukkan bahwa kontribusi pelanggan terhadap revenue tidak selalu sebanding dengan ukuran segmennya.

**Loyal Regulars** mencakup sekitar **27,6% pelanggan**, tetapi menghasilkan sekitar **49,4% revenue**.

Sementara itu, **Low-Value Browsers** mencakup sekitar **30,8% pelanggan**, tetapi hanya menghasilkan sekitar **7,5% revenue**.

---

### 3. Analisis Kunjungan Ulang

Pelanggan dikategorikan sebagai repeat visitor apabila memiliki lebih dari satu kunjungan.

Dari 500 pelanggan:

- **297 pelanggan (59,4%)** melakukan kunjungan ulang.
- **203 pelanggan (40,6%)** hanya melakukan satu kunjungan.

Untuk menganalisis faktor yang berkaitan dengan kunjungan ulang, karakteristik pada **kunjungan pertama** digunakan sebagai predictor.

Model yang digunakan:

- Logistic Regression
- Linear Discriminant Analysis (LDA)
- 5-fold Stratified Cross-Validation

Evaluasi cross-validation menunjukkan:

| Model | Accuracy | ROC-AUC |
|---|---:|---:|
| Logistic Regression | 60,4% | 0,526 |
| LDA | 60,6% | 0,527 |

Kedua model memberikan hasil yang hampir identik, sehingga **Logistic Regression** dipilih sebagai model yang lebih mudah diinterpretasikan.

Hasil model menunjukkan bahwa `first_spend` memiliki hubungan yang signifikan dengan repeat visit, sementara fitur first-visit lainnya tidak menunjukkan hubungan yang signifikan pada tingkat 5%.

Namun, kemampuan prediksi model secara keseluruhan masih **lemah**. Hal ini menunjukkan bahwa karakteristik kunjungan pertama saja belum cukup untuk menjelaskan perilaku kunjungan ulang secara akurat.

---

## 💰 Future High-Value Customer

Untuk menghindari data leakage, pelanggan bernilai tinggi didefinisikan berdasarkan **future spend**, yaitu total pengeluaran pelanggan setelah kunjungan pertama.

Pelanggan dengan future spend pada atau di atas **persentil ke-67** dikategorikan sebagai future high-value customer.

Hasilnya:

- **165 pelanggan (33,0%)** masuk kategori high-value.
- Threshold future spend: **9,8058**

Pendekatan ini memungkinkan analisis menggunakan informasi dari kunjungan pertama untuk memprioritaskan pelanggan berdasarkan potensi nilai mereka di masa berikutnya.

---

## 🎯 Propensity Scoring & Customer Targeting

Model Logistic Regression digunakan untuk menghasilkan **out-of-fold propensity score**, yaitu estimasi relatif mengenai kemungkinan seorang pelanggan masuk ke kelompok future high-value.

Hasil evaluasi menunjukkan:

- CV Accuracy: **65,8%**
- Baseline Accuracy: **67,0%**
- CV ROC-AUC: **0,538**

Karena performa model masih terbatas, propensity score **tidak digunakan sebagai prediksi absolut**, melainkan sebagai **sinyal prioritas relatif**.

Sebanyak **25% pelanggan dengan propensity score tertinggi** dipilih sebagai kelompok prioritas:

- **125 pelanggan**
- **25,0% dari total pelanggan**
- Threshold propensity: sekitar **0,400**
- Actual high-value rate: **35,2%**

Sebagai pembanding, actual high-value rate pada keseluruhan customer base adalah **33,0%**.

Artinya, kelompok Top 25% menunjukkan peningkatan sekitar **2,2 percentage points**, sehingga propensity score memberikan enrichment yang modest tetapi belum cukup kuat untuk digunakan sebagai satu-satunya dasar targeting.

---

## 💡 Key Findings

### 1. Tidak semua pelanggan memberikan kontribusi yang sama

**Loyal Regulars** hanya mencakup sekitar 27,6% pelanggan, tetapi berkontribusi sekitar 49,4% terhadap revenue.

Sebaliknya, **Low-Value Browsers** merupakan kelompok pelanggan terbesar, tetapi memberikan kontribusi revenue yang jauh lebih kecil.

Hal ini menunjukkan pentingnya menerapkan strategi yang berbeda berdasarkan karakteristik dan nilai pelanggan.

### 2. Repeat visit merupakan bagian penting dari customer base

Sebanyak **59,4% pelanggan melakukan kunjungan ulang**.

Namun, kelompok one-time customer yang mencapai 40,6% juga masih cukup besar sehingga terdapat peluang untuk meningkatkan customer retention.

### 3. First-visit behavior saja belum cukup untuk memprediksi retention

Logistic Regression dan LDA menghasilkan performa yang hampir sama dengan ROC-AUC sekitar 0,53.

Dengan demikian, faktor lain seperti recency, jarak ke kios, kebutuhan pelanggan, pengalaman layanan, dan engagement kemungkinan diperlukan untuk meningkatkan kemampuan prediksi.

### 4. Future high-value customer dapat diprioritaskan secara relatif

Propensity scoring dapat digunakan untuk membuat ranking prioritas pelanggan.

Namun, karena ROC-AUC model hanya sekitar 0,538, hasil propensity perlu diperlakukan sebagai **sinyal prioritas**, bukan sebagai prediksi yang sangat akurat.

---

## 🎯 Business Recommendations

Berdasarkan hasil analisis, beberapa strategi yang dapat dipertimbangkan:

### 1. Pertahankan Loyal Regulars

Kelompok ini memberikan kontribusi revenue terbesar.

Strategi dapat difokuskan pada:

- Loyalty program
- Membership
- Benefit untuk kunjungan rutin
- Cross-selling yang relevan

### 2. Ubah High-Value One-Timers menjadi pelanggan berulang

Pelanggan yang melakukan transaksi bernilai tinggi tetapi belum kembali merupakan peluang untuk meningkatkan retention.

Strategi yang dapat diuji:

- Follow-up setelah kunjungan pertama
- Penawaran membership
- Insentif untuk kunjungan berikutnya
- Produk atau layanan yang relevan dengan pembelian sebelumnya

### 3. Gunakan propensity score sebagai prioritas, bukan keputusan tunggal

Customer dengan propensity lebih tinggi dapat diprioritaskan untuk engagement atau follow-up.

Namun, keputusan targeting sebaiknya tetap mempertimbangkan faktor bisnis lainnya.

### 4. Uji strategi cross-selling

Karena karakteristik pembelian pertama memiliki hubungan dengan perilaku kunjungan ulang, perusahaan dapat menguji strategi seperti:

- Bundling
- Cross-selling
- Rekomendasi produk tambahan

Dampaknya perlu diukur melalui eksperimen agar tidak hanya mengandalkan hubungan statistik.

### 5. Tingkatkan kualitas data untuk analisis berikutnya

Model prediksi dapat dikembangkan dengan menambahkan fitur seperti:

- Recency
- Jarak pelanggan ke kios
- Riwayat engagement
- Interaksi dengan loyalty program
- Pola pembelian antar kunjungan
- Jenis produk yang dibeli
- Waktu antar kunjungan

---

## 🛠️ Tools & Technologies

### Tools

- Python
- Pandas
- NumPy
- Matplotlib
- Seaborn
- Scikit-learn
- Statsmodels

### Analytical Methods

- Exploratory Data Analysis
- Data Quality Analysis
- Customer-Level Aggregation
- K-Means Clustering
- Silhouette Analysis
- Logistic Regression
- Linear Discriminant Analysis (LDA)
- 5-Fold Cross-Validation
- Propensity Scoring
- Customer Targeting

---

## 📁 Repository Structure

```text
analisis-perilaku-pelanggan-kios-offline/
│
├── README.md
├── analisis_perilaku_pelanggan_kios_offline.ipynb
├── customers.csv
└── kiosk_visits.csv
