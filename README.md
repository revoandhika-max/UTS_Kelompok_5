# Implementasi Algoritma Decision Tree untuk Klasifikasi Tingkat Respons Getaran Struktur Bangunan Miniatur pada Simulasi Gempa

Proyek Machine Learning untuk Ujian Tengah Semester (UTS) dengan mengimplementasikan algoritma **Decision Tree Classifier** guna mengklasifikasikan tingkat  getaran struktur bangunan berdasarkan data sensor fisik untuk simulasi gempa.

## Struktur Pipeline 5 Sel:

Diagram alur di atas menggambarkan urutan proses sistem deteksi dan klasifikasi getaran berbasis sensor MPU6050 dari awal hingga akhir:

1. Start:
* Titik awal dimulainya eksekusi program 


2.  Data Loading (Membaca File CSV):
* Program memuat dataset mentah dari file `.csv` (`data_getaran_dummy.csv`) yang berisi catatan hasil percobaan sensor (percepatan dasar, percepatan struktur, dan perpindahan).


3.  Preprocessing & Labeling (Aturan Logika Displacement):**
* Tahap pembersihan dan penerjemahan data angka mentah perpindahan bangunan menggunakan fungsi berbasis aturan `if-elif-else` untuk memberikan label kelas target (`Gempa Rendah`, `Gempa Sedang`, `Gempa Tinggi`), serta memisahkan variabel Fitur (X) dan Target (y).


4.  Model Training (Decision Tree Classifier):
* Proses pelatihan komputer menggunakan algoritma **Decision Tree** dengan kriteria *Gini Impurity* dan batas kedalaman pohon (*max_depth*), di mana model mempelajari pola hubungan dari data sensor.
     * a.  `from sklearn.tree import DecisionTreeClassifier`
       * baris ini berfungsi untuk mengimpor algoritma Decision Tree Classifier dari library scikit-learn (sklearn). Algoritma ini digunakan khusus untuk melakukan tugas klasifikasi (mengelompokkan         data ke dalam kategori-kategori tertentu seperti Getaran Rendah, Sedang, atau Tinggi).
     * b.  `model = DecisionTreeClassifier(
            criterion="gini",
            max_depth=3,
            random_state=42
          )`
       *   `criterion="gini"`: Menentukan rumus metrik untuk mengukur "kebersihan" atau tingkat ketidakmurnian suatu data pada percabangan pohon menggunakan indeks Gini (Gini Impurity).

        *  `max_depth=3`: Membatasi kedalaman maksimal pohon keputusan hingga 3 tingkat ke bawah. Ini berfungsi agar pohon tidak terlalu panjang (menghindari overfitting atau hafalan berlebih).

         * `random_state=42`: Angka acak tetap (seed) agar setiap kali kode ini dijalankan ulang, hasil pemilihan acak dan bentuk pohon keputusannya selalu konsisten dan sama persis.

     * c. `model.fit(X, y)`
       * Fungsi `.fit()` memerintahkan komputer untuk "belajar" dari data latihan. Komputer akan mencocokkan pola dari variabel Fitur (X): yaitu percepatan dasar dan struktur—terhadap label                   Target dan (y):yaitu kelas gempanya. Sehingga model terbentuk dan siap digunakan untuk menebak data baru.


5.  Model Visualization (Plot Tree Gini & Rules):
* Menghasilkan diagram visualisasi pohon keputusan secara grafis guna menampilkan titik percabangan kondisi sensor, nilai *Gini*, dan keputusan kelas akhir secara transparan.
* a.Mengimpor Library Visualisasi & Plotting

    import matplotlib.pyplot as plt: Memanggil library Matplotlib yang berfungsi untuk merender dan menampilkan grafik atau gambar di dalam Jupyter Notebook.

  from sklearn.tree import plot_tree: Mengimpor fungsi khusus plot_tree dari scikit-learn untuk mengubah model Decision Tree yang sudah dilatih menjadi diagram visual berbentuk pohon secara         otomatis.


6.  Inference / Prediksi (Input Sensor Baru):**
* Tahap pengujian akhir di mana pengguna dapat memasukkan nilai input percepatan sensor secara interaktif untuk langsung mendapatkan hasil prediksi kategori status gempa secara *real-time*.
