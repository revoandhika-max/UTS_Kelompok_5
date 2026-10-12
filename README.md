# Implementasi Algoritma Decision Tree untuk Klasifikasi Tingkat Respons Getaran Struktur Bangunan Miniatur pada Simulasi Gempa

Proyek Machine Learning untuk Ujian Tengah Semester (UTS) dengan mengimplementasikan algoritma **Decision Tree Classifier** guna mengklasifikasikan tingkat  getaran struktur bangunan berdasarkan data sensor fisik untuk simulasi gempa.

## Struktur Pipeline 5 Sel:

Diagram alur di atas menggambarkan urutan proses sistem deteksi dan klasifikasi getaran berbasis sensor MPU6050 dari awal hingga akhir:

1. **Start:**
* Titik awal dimulainya eksekusi program 


2.  Data Loading (Membaca File CSV):**
* Program memuat dataset mentah dari file `.csv` (`data_getaran_dummy.csv`) yang berisi catatan hasil percobaan sensor (percepatan dasar, percepatan struktur, dan perpindahan).


3.  Preprocessing & Labeling (Aturan Logika Displacement):**
* Tahap pembersihan dan penerjemahan data angka mentah perpindahan bangunan menggunakan fungsi berbasis aturan `if-elif-else` untuk memberikan label kelas target (`Gempa Rendah`, `Gempa Sedang`, `Gempa Tinggi`), serta memisahkan variabel Fitur (X) dan Target (y).


4.  Model Training (Decision Tree Classifier):**
* Proses pelatihan komputer menggunakan algoritma **Decision Tree** dengan kriteria *Gini Impurity* dan batas kedalaman pohon (*max_depth*), di mana model mempelajari pola hubungan dari data sensor.


5.  Model Visualization (Plot Tree Gini & Rules):**
* Menghasilkan diagram visualisasi pohon keputusan secara grafis guna menampilkan titik percabangan kondisi sensor, nilai *Gini*, dan keputusan kelas akhir secara transparan.


6.  Inference / Prediksi (Input Sensor Baru):**
* Tahap pengujian akhir di mana pengguna dapat memasukkan nilai input percepatan sensor secara interaktif untuk langsung mendapatkan hasil prediksi kategori status gempa secara *real-time*.
