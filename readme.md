# Mini-Project: Deteksi Tanda Tangan (Signature Detection)

**Mata Kuliah:** Pengolahan Citra Digital  
**Nama:** Salwa  
**NIM:** F1G124017  
**Kelas:** Kelas C  

---

## 📌 Deskripsi Proyek
Proyek ini merupakan implementasi pengolahan citra digital berbasis **Document Image Analysis (DIA)** menggunakan Python dan OpenCV. Sistem ini dirancang untuk melakukan verifikasi keberadaan tanda tangan pada dokumen resmi secara otomatis dengan mengisolasi Region of Interest (ROI), memproses citra melalui binerisasi thresholding, menyempurnakan bentuk dengan operasi morfologi, serta menghitung rasio piksel tinta (*foreground*) sebagai aturan keputusan.

---

## 📁 Struktur Repositori
```
.
├── data/                             # Direktori sampel citra dokumen pengujain
├── Tugas_Citra_Digital.ipynb         # Jupyter Notebook berisi seluruh kode & eksperimen
└── README.md                         # Dokumentasi lengkap proyek
```

---

## ⚙️ Alur Pemrosesan (Pipeline)
1. **Region of Interest (ROI) Cropping:** Memotong area spesifik pada dokumen tempat tanda tangan berada berdasarkan proporsi persentase resolusi citra agar adaptif.
2. **Grayscale Conversion:** Mengubah citra warna (RGB) menjadi skala keabuan (*grayscale*).
3. **Thresholding (Global vs Otsu):** Mengubah citra *grayscale* menjadi biner (hitam-putih) untuk memisahkan goresan tinta (*foreground*) dari latar belakang kertas (*background*). Otsu dipilih karena kemampuannya menentukan nilai ambang secara otomatis berdasarkan histogram piksel.
4. **Morphological Operations:**
   - **Opening** (Erosi $ightarrow$ Dilasi): Menghilangkan *noise* atau bintik kotoran pada kertas.
   - **Closing** (Dilasi $ightarrow$ Erosi): Menyambungkan goresan tinta tanda tangan yang terputus akibat pindaian.
5. **Pixel Ratio Calculation & Decision Rule:** Menhitung rasio persentase piksel tinta terhadap total piksel area ROI.
   - **Jika Pixel Ratio $\ge$ 1.5%:** $ightarrow$ `SIGNATURE PRESENT`
   - **Jika Pixel Ratio < 1.5%:** $ightarrow$ `SIGNATURE ABSENT`

---

## 📊 Hasil Pengujian & Evaluasi

| Nama Sampel | Otsu Threshold | Pixel Ratio (%) | Status Deteksi |
| :--- | :---: | :---: | :---: |
| `01_HighQuality_Enhanced` | 158 | 7.41% | **SIGNATURE PRESENT** |
| `02_LowContrast` | 142 | 7.50% | **SIGNATURE PRESENT** |
| `03_Blurred` | 135 | 11.69% | **SIGNATURE PRESENT** |
| `04_HighNoise` | 160 | 7.16% | **SIGNATURE PRESENT** |
| `05_LowResolution_Upsampled` | 150 | 9.20% | **SIGNATURE PRESENT** |
| `06_Faded_Underexposed` | 128 | 7.63% | **SIGNATURE PRESENT** |
| `07_ColorShift_WarmTint` | 152 | 7.52% | **SIGNATURE PRESENT** |
| `08_JPEGCompression_Artifacts` | 155 | 7.72% | **SIGNATURE PRESENT** |
| `09_CombinedDegradation` | 148 | 8.81% | **SIGNATURE PRESENT** |
| `10_Blank_Document` | 210 | 0.12% | **SIGNATURE ABSENT** |

---

## 🧠 Analisis & Jawaban Pertanyaan

### 1. Mengapa thresholding diperlukan sebelum melakukan analisis keberadaan tanda tangan?
Komputer tidak mengenali bentuk visual tanda tangan seperti manusia, melainkan hanya melihat matriks intensitas piksel (0–255). *Thresholding* mutlak diperlukan untuk mengubah citra *grayscale* menjadi citra biner secara tegas. Proses ini memisahkan goresan tinta (*foreground*) dari warna kertas/watermark (*background*), sehingga sistem dapat melakukan kalkulasi luasan piksel objek secara matematis untuk mengambil keputusan deteksi.

### 2. Apa masalah yang terjadi jika threshold terlalu tinggi atau terlalu rendah?
- **Threshold Terlalu Rendah:** Sistem mengabaikan piksel yang tidak cukup gelap. Goresan tinta tipis atau pudar akan dianggap sebagai kertas putih, menyebabkan **False Negative** (tanda tangan ada tetapi terdeteksi tidak ada).
- **Threshold Terlalu Tinggi:** Sistem menjadi terlalu sensitif. Elemen latar belakang seperti bayangan, watermark, atau noise pindaian akan dianggap sebagai tinta, menyebabkan **False Positive** (dokumen kosong terdeteksi memiliki tanda tangan).
- **Solusi:** Penggunaan **Otsu Thresholding** dikombinasikan dengan **Operasi Morfologi** terbukti stabil dalam mengatasi variasi pencahayaan dan degradasi citra.

---

## 🚀 Panduan Menjalankan Kode (*How to Run*)

### 1. Prasyarat System
Pastikan Python 3.x dan library berikut telah terinstall:
```bash
pip install opencv-python numpy matplotlib
```

### 2. Menjalankan Jupyter Notebook
1. Clone repositori ini ke komputer lokal Anda:
   ```bash
   git clone https://github.com/Salwa/miniproject-signature-detection.git
   ```
2. Jalankan Jupyter Notebook:
   ```bash
   jupyter notebook Tugas_Citra_Digital.ipynb
   ```
3. Eksekusi seluruh cell secara berurutan (`Shift + Enter`).
