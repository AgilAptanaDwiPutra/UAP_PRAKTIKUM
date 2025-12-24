# 🎵 Analisis Tren Musik Populer di spotify berdasarkan Pola popularity

## 📌 Deskripsi Proyek
Proyek ini bertujuan untuk melakukan **analisis tren musik populer di Spotify** serta membangun **sistem prediksi popularitas lagu** menggunakan pendekatan *deep learning* dan *Transfer Learning*.

Popularitas lagu pada platform streaming tidak hanya dipengaruhi oleh karakteristik audio, tetapi juga oleh metadata lagu. Oleh karena itu, proyek ini mengombinasikan **analisis data eksploratif (EDA)** dan **pemodelan prediktif** untuk memahami pola tren musik serta memprediksi tingkat popularitas lagu berdasarkan fitur-fitur yang tersedia.

Sebagai hasil akhir, proyek ini dilengkapi dengan **dashboard web berbasis Streamlit** yang memungkinkan pengguna melakukan prediksi popularitas lagu secara interaktif.

---

## 📂 Dataset dan Preprocessing

### 📊 Dataset
Dataset yang digunakan merupakan dataset musik Spotify yang terdiri dari lebih dari **114.000 lagu**, dengan atribut utama sebagai berikut:

**Target:**
- `popularity` (nilai numerik 0–100)

**Fitur Audio (Numerik):**
- `duration_ms`
- `danceability`
- `energy`
- `key`
- `loudness`
- `mode`
- `speechiness`
- `acousticness`
- `instrumentalness`
- `liveness`
- `valence`
- `tempo`
- `time_signature`

**Fitur Metadata (Kategorikal):**
- `track_genre`
- `artists` (diolah menjadi `artists_top`)
- `explicit`

---

### ⚙️ Tahapan Preprocessing
Tahapan preprocessing yang dilakukan dalam proyek ini meliputi:
1. Penghapusan kolom dengan kardinalitas tinggi seperti `track_id`, `track_name`, dan `album_name`.
2. Normalisasi fitur numerik menggunakan **StandardScaler**.
3. Encoding fitur kategorikal menggunakan ordinal encoding.
4. Pengelompokan artis menggunakan pendekatan **Top-K Encoding**, di mana artis dengan frekuensi rendah dikelompokkan ke dalam kategori `__OTHER__`.
5. Pembagian dataset ke dalam data **train**, **validation**, dan **test** untuk mencegah data leakage.

---

## 🤖 Model yang Digunakan

Dalam proyek ini digunakan **tiga model utama** dengan pendekatan yang berbeda, yaitu:

### 1️⃣ Feedforward Neural Network (FNN)
- Digunakan sebagai **model klasifikasi**
- Target popularitas dikategorikan menjadi dua kelas: *Popular* dan *Not Popular*
- Berfungsi sebagai *baseline model*
- Digunakan untuk mengevaluasi kemampuan neural network sederhana dalam memprediksi popularitas lagu

---

### 2️⃣ TabNet (Regression)
- Digunakan sebagai **model regresi**
- Model tabular modern yang memiliki mekanisme *feature selection* internal
- Digunakan untuk menguji kemampuan model tabular dalam memodelkan hubungan antara fitur audio dan popularitas lagu

---

### 3️⃣ FT-Transformer (Regression)
- Model utama dalam proyek ini
- Mengadaptasi arsitektur Transformer untuk data tabular
- Mampu menangkap hubungan non-linear antara fitur audio dan metadata
- Diimplementasikan ke dalam **dashboard web Streamlit** sebagai sistem prediksi akhir

---

## 📈 Hasil Evaluasi dan Analisis Perbandingan

| Nama Model | Performa Utama | Analisis |
|-----------|----------------|----------|
| **FNN (Classification)** | Accuracy = **0.75** | Model mampu mengklasifikasikan lagu *Not Popular* dengan baik, namun memiliki performa rendah dalam mendeteksi lagu *Popular*. Hal ini menunjukkan adanya bias terhadap kelas mayoritas. |
| **TabNet (Regression)** | RMSE = **20.86**<br>MAE = **16.80**<br>R² = **0.12** | Model hanya mampu menjelaskan sebagian kecil variansi popularitas lagu. Kurang optimal dalam menangkap hubungan kompleks antar fitur. |
| **FT-Transformer (Regression)** | RMSE = **15.85**<br>MAE = **10.18**<br>R² = **0.49** | Model dengan performa terbaik. Mampu memodelkan hubungan kompleks antara fitur audio dan metadata serta memberikan prediksi yang lebih akurat dan stabil. |

**Kesimpulan:**  
Model **FT-Transformer** menunjukkan performa paling unggul dibandingkan FNN dan TabNet, sehingga dipilih sebagai model final dalam sistem prediksi popularitas lagu.

---

## 🌐 Panduan Menjalankan Sistem Website Secara Lokal

### Prasyarat
- Python versi **3.10 atau lebih baru**
- **PDM (Python Dependency Manager)**

### LOAD DATA
- Model sudah diupload lalu bisa didownload untuk di load ke streamlit

### Instalasi Dependency
Jalankan perintah berikut pada root project:

```bash
pdm install

##▶️ Menjalankan Dashboard Streamlit
Pastikan file model hasil training (.pt) berada di folder artifacts/, kemudian jalankan:
pdm run streamlit run src/app.py
