# Perbandingan Kombinasi Fitur Piksel Mentah, GLCM, HOG, dan LBP secara Bertahap pada Klasifikasi Penyakit Daun Tomat Menggunakan SVM

## Deskripsi Penelitian

Repositori ini berisi source code, notebook eksperimen, artefak model, dan berkas pendukung aplikasi untuk penelitian klasifikasi penyakit daun tomat menggunakan algoritma **Support Vector Machine (SVM)**.

Penelitian ini bertujuan membandingkan kinerja model SVM berdasarkan penambahan fitur citra secara bertahap, yaitu Raw Pixel, Gray-Level Co-occurrence Matrix (GLCM), Histogram of Oriented Gradients (HOG), dan Local Binary Pattern (LBP).

Tiga konfigurasi model yang digunakan adalah:

| Model | Kombinasi Fitur | Dimensi Fitur |
|---|---|---:|
| M1 | Raw Pixel | 3.072 |
| M2 | Raw Pixel + GLCM | 3.132 |
| M3 | Raw Pixel + GLCM + HOG + LBP | 3.466 |

Ketiga model menggunakan kernel Radial Basis Function (RBF). Penelitian mencakup pelatihan model, pencarian parameter, evaluasi holdout test, cross-validation, dan pengujian tambahan terhadap variasi pencahayaan dan kontras.

## Dataset

Dataset yang digunakan berasal dari PlantVillage dan terdiri atas empat kelas citra daun tomat:

| Kelas | Jumlah Citra |
|---|---:|
| Early Blight | 1.000 |
| Healthy | 1.591 |
| Late Blight | 1.909 |
| Septoria Leaf Spot | 1.771 |
| **Total** | **6.271** |

Pembagian dataset menggunakan rasio 80:20 dengan `random_state=42`.

| Pembagian | Jumlah Citra |
|---|---:|
| Data latih | 5.016 |
| Data pengujian | 1.255 |
| **Total** | **6.271** |

Dataset tidak disertakan dalam repositori ini. Pengguna perlu menyediakan dataset secara terpisah dan menyesuaikan lokasi direktori dataset pada notebook atau source code.

## Metode Penelitian

### 1. Pra-pemrosesan Citra

Pra-pemrosesan dilakukan untuk menyiapkan citra sebelum ekstraksi fitur, meliputi pembacaan citra, penyesuaian ukuran, konversi ruang warna sesuai kebutuhan metode ekstraksi, serta penskalaan fitur menggunakan `RobustScaler`.

Penskalaan dilakukan secara terpisah untuk setiap kelompok fitur. Parameter scaler diperoleh dari data latih.

### 2. Ekstraksi Fitur

**a. Raw Pixel**

Citra diubah ukurannya menjadi 32 × 32 piksel dengan tiga kanal RGB, kemudian diratakan menjadi vektor berukuran 3.072 fitur.

**b. Gray-Level Co-occurrence Matrix (GLCM)**

GLCM digunakan untuk memperoleh informasi tekstur berdasarkan hubungan spasial antarpiksel. Konfigurasi yang digunakan:

- Tingkat keabuan: 64.
- Jarak: 1, 2, dan 3.
- Sudut: 0°, 45°, 90°, dan 135°.
- Properti: Energy, Contrast, Correlation, Homogeneity, dan Entropy.

Jumlah fitur yang dihasilkan adalah 60.

**c. Histogram of Oriented Gradients (HOG)**

HOG digunakan untuk merepresentasikan distribusi orientasi gradien pada citra. Konfigurasi yang digunakan:

- Ukuran citra: 128 × 128 piksel.
- Orientasi: 9.
- Ukuran sel: 32 × 32 piksel.
- Ukuran blok: 2 × 2 sel.
- Normalisasi: L2-Hys.

Jumlah fitur yang dihasilkan adalah 324.

**d. Local Binary Pattern (LBP)**

LBP digunakan untuk merepresentasikan pola tekstur lokal dengan konfigurasi:

- Jumlah titik tetangga (P): 8.
- Radius (R): 1.
- Metode: Uniform.
- Representasi: Histogram ternormalisasi.

Jumlah fitur yang dihasilkan adalah 10.

### 3. Pembentukan Model SVM

Ketiga model menggunakan kernel RBF. Pencarian parameter dilakukan menggunakan `GridSearchCV` dengan ruang pencarian:

| Parameter | Nilai yang Diuji |
|---|---|
| C | 1, 10, 100 |
| Gamma | `scale`, 0.01, 0.001 |
| Kernel | RBF |

Konfigurasi terbaik yang diperoleh untuk ketiga model adalah C = 10 dan gamma = `scale`.

Nilai gamma aktual yang terbaca dari model terlatih:

| Model | Gamma Setelan | Gamma Aktual |
|---|---|---:|
| M1 | `scale` | 3,04 × 10⁻⁴ |
| M2 | `scale` | 2,93 × 10⁻⁴ |
| M3 | `scale` | 2,77 × 10⁻⁴ |

### 4. Evaluasi Model

Evaluasi dilakukan menggunakan:

- **Holdout Test:** mengukur performa pada data pengujian.
- **Cross-Validation:** menggunakan StratifiedGroupKFold dengan 3 fold.
- **Robustness Check:** pengujian tambahan untuk mengamati performa model terhadap variasi pencahayaan dan kontras.

Metrik evaluasi meliputi Accuracy, Precision, Recall, dan F1-Score.

## Hasil Penelitian

### 1. Holdout Test

| Metrik | M1 | M2 | M3 |
|---|---:|---:|---:|
| Accuracy | 88,45% | 90,36% | 92,35% |
| Macro Precision | 87,26% | 89,28% | 91,68% |
| Macro Recall | 86,67% | 88,86% | 90,71% |
| Macro F1-Score | 86,93% | 89,04% | 91,12% |

### 2. Cross-Validation

| Model | Rata-rata Accuracy |
|---|---:|
| M1 | 84,39% |
| M2 | 86,58% |
| M3 | 89,49% |

### 3. Robustness Check

Pengujian tambahan dilakukan sebanyak tiga putaran.

| Model | Accuracy (Rata-rata ± Standar Deviasi) | Robustness F1-Score |
|---|---:|---:|
| M1 | 87,07 ± 1,10% | 85,74% |
| M2 | 88,85 ± 0,63% | 87,53% |
| M3 | 91,13 ± 0,36% | 90,12% |

M3 memperoleh performa tertinggi pada ketiga rangkaian evaluasi tersebut. Hasil ini menunjukkan bahwa konfigurasi fitur M3 memberikan kinerja klasifikasi terbaik dalam eksperimen yang dilakukan. Namun, kontribusi HOG dan LBP belum dapat dipisahkan karena keduanya ditambahkan secara bersamaan pada M3.

## Struktur Repositori

```text
perbandingan-fitur-svm-penyakit-daun-tomat/
│
├── README.md
├── requirements.txt
│
├── notebooks/
│   ├── SVM_Bertahapp.ipynb
│   └── UI_Aplikasi_Perbandingan.ipynb
│
├── scripts/
│   └── Nilai_Gamma_Model.py
│
├── verification/
│   └── Verif_Aplikasi.py
│
├── artifacts/
│   ├── svm_m1_raw.pkl
│   ├── svm_m2_raw_glcm.pkl
│   ├── svm_m3_raw_glcm_hog_lbp.pkl
│   ├── raw_scaler.pkl
│   ├── glcm_scaler.pkl
│   ├── hog_scaler.pkl
│   ├── lbp_scaler.pkl
│   ├── feature_config.json
│   └── feature_utils.py
│
├── verification_results/
│   ├── Verif_aplikasi/
│   └── Verifikasi_UI/
│       ├── U01/
│       ├── U02/
│       ├── ...
│       └── U14/
│
└── Dokumentasi/
    └── DAFTAR_PUSTAKA.pdf
```

**Keterangan direktori:**

| Direktori | Keterangan |
|---|---|
| `notebooks/` | Notebook eksperimen dan notebook aplikasi perbandingan. |
| `scripts/` | Skrip pendukung untuk membaca informasi parameter model. |
| `verification/` | Source code pendukung proses verifikasi aplikasi. |
| `artifacts/` | Model SVM, scaler, konfigurasi fitur, dan komponen pendukung inferensi. |
| `verification_results/Verif_aplikasi/` | Direktori untuk hasil keluaran verifikasi aplikasi. |
| `verification_results/Verifikasi_UI/` | Hasil verifikasi antarmuka U01–U14. |
| `Dokumentasi/` | Dokumen pendukung penelitian. |

Nama dan susunan file pada struktur di atas perlu disesuaikan dengan berkas yang benar-benar tersedia di repositori.

## Source Code Tambahan

### 1. Nilai_Gamma_Model.py

Skrip `Nilai_Gamma_Model.py` digunakan untuk memeriksa keberadaan berkas model SVM M1, M2, dan M3, memuat model menggunakan Joblib, serta mengambil informasi jumlah fitur, parameter C, konfigurasi gamma, dan nilai gamma aktual (`_gamma`).

Skrip ini juga menyediakan fungsi untuk memformat nilai gamma ke dalam notasi ilmiah.

Nilai gamma aktual yang diperoleh:

| Model | Dimensi Fitur | C | Gamma Setelan | Gamma Aktual |
|---|---:|---:|---|---:|
| M1 | 3.072 | 10 | `scale` | 3,04 × 10⁻⁴ |
| M2 | 3.132 | 10 | `scale` | 2,93 × 10⁻⁴ |
| M3 | 3.466 | 10 | `scale` | 2,77 × 10⁻⁴ |

### 2. Verif_Aplikasi

`Verif_Aplikasi` merupakan source code pendukung proses verifikasi aplikasi. Skrip ini menggunakan beberapa direktori untuk membaca dataset, artefak model, dan hasil verifikasi sebelumnya, serta menentukan lokasi penyimpanan hasil verifikasi baru.

Konfigurasi direktori yang digunakan dalam lingkungan pengembangan:

- `DATASET_PATH`: lokasi dataset PlantVillage.
- `MODEL_DIR`: lokasi model dan komponen pendukung di folder `SVM_bertahap`.
- `KELAS_LAIN_DIR`: lokasi dataset kelas lain, yaitu `Tomato___Target_Spot`.
- `VERIF_LAMA_DIR`: lokasi hasil verifikasi antarmuka sebelumnya, yaitu `SVM_bertahap/Verifikasi_UI`.
- `OUTPUT_DIR` atau `VERIF_DIR`: lokasi keluaran verifikasi baru di folder `Verif_aplikasi`.

Folder dataset dan hasil verifikasi lama digunakan sebagai sumber masukan dalam proses verifikasi. Hasil verifikasi baru disimpan pada direktori keluaran yang ditentukan oleh source code.

## Aplikasi Klasifikasi

Repositori ini menyertakan notebook aplikasi perbandingan untuk menggunakan model SVM yang telah dilatih.

Aplikasi digunakan untuk melakukan klasifikasi citra daun tomat menggunakan tiga konfigurasi model, yaitu M1, M2, dan M3, serta membandingkan hasil prediksi antarmodel.

Model dan komponen pendukung dibaca dari artefak hasil eksperimen. Proses pelatihan model dilakukan pada notebook eksperimen, bukan pada saat penggunaan model untuk inferensi.

Model dibangun untuk empat kelas, yaitu Early Blight, Healthy, Late Blight, dan Septoria Leaf Spot. Oleh karena itu, hasil prediksi pada citra di luar keempat kelas tersebut tidak dapat dianggap sebagai identifikasi yang valid.

## Instalasi dan Penggunaan

### 1. Persiapan

Pastikan perangkat telah memiliki:

- Python.
- pip.
- Git, jika repositori akan diunduh menggunakan Git.

### 2. Mengunduh Repositori

Jalankan perintah berikut pada terminal:

```bash
git clone https://github.com/nabillakesuma/perbandingan-fitur-svm-penyakit-daun-tomat.git
```

Masuk ke direktori repositori:

```bash
cd perbandingan-fitur-svm-penyakit-daun-tomat
```

### 3. Instalasi Dependensi

Instal paket yang tercantum dalam `requirements.txt`:

```bash
pip install -r requirements.txt
```

### 4. Menjalankan Notebook Eksperimen

Buka notebook:

```text
notebooks/SVM_Bertahapp.ipynb
```

Notebook ini digunakan untuk menjalankan tahapan eksperimen, ekstraksi fitur, pelatihan model, pencarian parameter, dan evaluasi.

Dataset harus disiapkan secara terpisah. Sesuaikan variabel lokasi dataset pada notebook dengan direktori dataset di perangkat yang digunakan.

### 5. Menjalankan Notebook Aplikasi

Buka notebook:

```text
notebooks/UI_Aplikasi_Perbandingan.ipynb
```

Pastikan artefak model dan komponen pendukung tersedia pada direktori yang sesuai dengan konfigurasi source code aplikasi.

### 6. Menjalankan Skrip Pendukung

Skrip pemeriksaan nilai gamma dapat dijalankan menggunakan Python:

```bash
python scripts/Nilai_Gamma_Model.py
```

Pastikan lokasi `ARTIFACT_DIR` pada skrip sudah disesuaikan dengan lokasi folder model di perangkat yang digunakan.

## Batasan Penelitian

1. Dataset yang digunakan berasal dari PlantVillage dan terdiri atas empat kelas penyakit atau kondisi daun tomat.
2. Dataset tidak menyediakan identitas tanaman atau daun untuk pembagian data berdasarkan individu.
3. Pengujian robustness merupakan evaluasi tambahan pada dataset penelitian, bukan validasi eksternal menggunakan dataset independen.
4. Penelitian belum melakukan pengujian signifikansi statistik untuk memastikan perbedaan performa antarmodel.
5. HOG dan LBP ditambahkan secara bersamaan pada M3 sehingga kontribusi masing-masing fitur belum dapat dipisahkan.
6. Pengujian pada citra lapangan dengan kondisi pengambilan gambar yang beragam belum dilakukan.

## Referensi

1. Cortes, C., & Vapnik, V. (1995). Support-vector networks. *Machine Learning, 20*, 273–297.
2. Dalal, N., & Triggs, B. (2005). Histograms of oriented gradients for human detection. *Proceedings of the IEEE Computer Society Conference on Computer Vision and Pattern Recognition*.
3. Mohanty, S. P., Hughes, D. P., & Salathé, M. (2016). Using deep learning for image-based plant disease detection. *Frontiers in Plant Science, 7*, 1419.
4. Ojala, T., Pietikäinen, M., & Mäenpää, T. (2002). Multiresolution gray-scale and rotation invariant texture classification with local binary patterns. *IEEE Transactions on Pattern Analysis and Machine Intelligence, 24*(7), 971–987.

## Pengembang

Repositori ini dikembangkan sebagai bagian dari penelitian tugas akhir Program Studi Teknik Informatika, Fakultas Ilmu Komputer, Universitas Sriwijaya.

**Topik penelitian:** Klasifikasi Penyakit Daun Tomat Menggunakan SVM dan Kombinasi Fitur Citra.
