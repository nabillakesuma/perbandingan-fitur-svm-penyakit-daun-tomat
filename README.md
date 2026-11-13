
# Perbandingan Kombinasi Fitur Piksel Mentah, GLCM, HOG, dan LBP pada Klasifikasi Penyakit Daun Tomat Menggunakan SVM

## Deskripsi

Repository ini berisi source code dan dokumentasi penelitian mengenai perbandingan kombinasi fitur citra pada klasifikasi penyakit daun tomat menggunakan algoritma Support Vector Machine (SVM).

Penelitian membandingkan penggunaan fitur piksel mentah (Raw Pixel), Gray Level Co-occurrence Matrix (GLCM), Histogram of Oriented Gradients (HOG), dan Local Binary Pattern (LBP).

## Dataset

Dataset yang digunakan adalah PlantVillage dengan empat kelas citra daun tomat:

| Kelas | Jumlah Citra |
|---|---:|
| Early Blight | 1.000 |
| Healthy | 1.591 |
| Late Blight | 1.909 |
| Septoria Leaf Spot | 1.771 |
| **Total** | **6.271** |

Pembagian data menggunakan rasio 80:20 dengan random_state=42.

Dataset tidak disertakan dalam repository dan perlu diperoleh secara terpisah.

## Metode Penelitian

Penelitian membandingkan tiga konfigurasi model:

| Model | Kombinasi Fitur |
|---|---|
| M1 | SVM + Raw Pixel |
| M2 | SVM + Raw Pixel + GLCM |
| M3 | SVM + Raw Pixel + GLCM + HOG + LBP |

Model menggunakan kernel RBF dengan optimasi parameter menggunakan GridSearchCV.

## Hasil Eksperimen

| Model | Accuracy |
|---|---:|
| M1 | 88,45% |
| M2 | 90,36% |
| M3 | 92,35% |

Hasil tersebut merupakan hasil evaluasi pada data pengujian penelitian.

## Struktur Repository

- `notebooks/`: notebook eksperimen dan aplikasi.
- `scripts/`: script Python pendukung penelitian.
- `artifacts/`: dokumentasi model dan komponen pendukung.
- `verification/`: dokumentasi hasil pengujian aplikasi.
- `docs/`: dokumentasi tambahan penelitian.

## Model SVM

File model M1, M2, dan M3 tidak disimpan langsung di GitHub karena ukuran file.

Model dapat diunduh melalui Google Drive:

**[Unduh Model SVM M1, M2, dan M3](MASUKKAN_LINK_GOOGLE_DRIVE)**

Pastikan pengguna memiliki akses untuk mengunduh ketiga file model tersebut.

## Instalasi

Install pustaka yang dibutuhkan menggunakan:

```bash
pip install -r requirements.txt
```

## Menjalankan Notebook

Buka notebook pada folder `notebooks/` menggunakan Jupyter Notebook atau Visual Studio Code.

Sesuaikan lokasi dataset dan model pada kode dengan struktur direktori di komputer masing-masing.

## Catatan

Repository ini disediakan untuk dokumentasi source code dan pendukung penelitian tugas akhir klasifikasi penyakit daun tomat menggunakan SVM.
