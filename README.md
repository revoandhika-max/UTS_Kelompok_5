# Implementasi Algoritma Decision Tree untuk Klasifikasi Tingkat Respons Getaran Struktur Bangunan Miniatur pada Simulasi Gempa

Proyek Machine Learning untuk Ujian Tengah Semester (UTS) dengan mengimplementasikan algoritma **Decision Tree Classifier** guna mengklasifikasikan tingkat  getaran struktur bangunan berdasarkan data sensor fisik untuk simulasi gempa.

## Struktur Pipeline 5 Sel:

Diagram alur di atas menggambarkan urutan proses sistem deteksi dan klasifikasi getaran berbasis sensor MPU6050 dari awal hingga akhir:

1. Start:
* Titik awal dimulainya eksekusi program 


2.  Data Loading (Membaca File CSV):

* Program memuat dataset mentah dari file `.csv` (`data_getaran_dummy.csv`) yang berisi catatan hasil percobaan sensor (percepatan dasar, percepatan struktur, dan perpindahan).


3.  Preprocessing & Labeling (Aturan Logika Displacement):
  
* Tahap pembersihan dan penerjemahan data angka mentah perpindahan bangunan menggunakan fungsi berbasis aturan `if-elif-else` untuk memberikan label kelas target (`Rendah`, `Sedang`, `Tinggi`), serta memisahkan variabel Fitur (X) dan Target (y).


4.  Model Training (Decision Tree Classifier):

* Proses pelatihan komputer menggunakan algoritma **Decision Tree** dengan kriteria *Gini Impurity* dan batas kedalaman pohon (*max_depth*), di mana model mempelajari pola hubungan dari data sensor.
     * a.  `from sklearn.tree import DecisionTreeClassifier`
       * baris ini berfungsi untuk mengimpor algoritma Decision Tree Classifier dari library scikit-learn (sklearn). Algoritma ini digunakan khusus untuk melakukan tugas klasifikasi (mengelompokkan         data ke dalam kategori-kategori tertentu seperti Rendah, Sedang, atau Tinggi).
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

        `import matplotlib.pyplot as plt`: Memanggil library Matplotlib yang berfungsi untuk merender dan menampilkan grafik atau gambar di dalam Jupyter Notebook.

        `from sklearn.tree import plot_tree`: Mengimpor fungsi khusus plot_tree dari scikit-learn untuk mengubah model Decision Tree yang sudah dilatih menjadi diagram visual berbentuk pohon secara           otomatis.
    * b. Mengatur Ukuran Kanvas Gambar

        `plt.figure(figsize=(10, 6))`: Berfungsi untuk menyiapkan ukuran kanvas (figure) grafik dengan lebar 10 inci dan tinggi 6 inci agar diagram pohon keputusan terlihat cukup luas, rapi, dan              tidak berhimpitan saat ditampilkan.
    * c. Fungsi Utama Pembuatan Diagram Pohon (plot_tree)

        `model`: Memasukkan objek model Decision Tree yang sebelumnya sudah dilatih menggunakan perintah `.fit(X, y)`.
        
        `feature_names=fitur`: Memberikan nama-nama kolom fitur (yaitu Percepatan dasar (g) dan Percepatan struktur (g)) agar teks pertanyaan di dalam kotak pohon sesuai dengan nama data aslinya.
        
        `class_names=model.classes_`: Menentukan label nama kelas target (Getaran Rendah, Getaran Sedang, Getaran Tinggi) agar kotak keputusan akhir menampilkan kategori bencana yang jelas.
        
        `filled=True`: Mewarnai kotak-kotak keputusan secara otomatis berdasarkan tingkat kemurnian atau kelas dominannya agar diagram jauh lebih mudah dibaca secara visual.
        
        `rounded=True`: Membuat sudut-sudut pada kotak diagram menjadi melengkung (rounded), sehingga tampilannya terlihat lebih modern dan rapi.
        
        `fontsize=10`: Mengatur ukuran teks di dalam diagram agar pas dan nyaman dibaca.
    


6.  Inference / Prediksi (Input Sensor Baru):
  
* Tahap pengujian akhir di mana pengguna dapat memasukkan nilai input percepatan sensor secara interaktif untuk langsung mendapatkan hasil prediksi kategori status gempa secara *real-time*.
