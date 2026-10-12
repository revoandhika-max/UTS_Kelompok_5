# Implementasi Algoritma Decision Tree untuk Klasifikasi Tingkat Respons Getaran Struktur Bangunan Miniatur pada Simulasi Gempa

Proyek Machine Learning untuk Ujian Tengah Semester (UTS) dengan mengimplementasikan algoritma **Decision Tree Classifier** guna mengklasifikasikan tingkat  getaran struktur bangunan berdasarkan data sensor fisik untuk simulasi gempa.

## Struktur Pipeline 5 Sel:

Diagram alur di atas menggambarkan urutan proses sistem deteksi dan klasifikasi getaran berbasis sensor MPU6050 dari awal hingga akhir:

1. Start:
* Titik awal dimulainya eksekusi program 


2.  Data Loading (Membaca File CSV):

* `import pandas as pd

# 1. Membaca file dataset CSV
df = pd.read_csv("data_getaran_dummy.csv")

# Menampilkan 5 baris pertama data
print("=== 5 DATA PERTAMA ===")
display(df.head(5))`

* Program memuat dataset mentah dari file `.csv` (`data_getaran_dummy.csv`) yang berisi catatan hasil percobaan sensor (percepatan dasar, percepatan struktur, dan perpindahan).


3.  Preprocessing & Labeling (Aturan Logika Displacement):

* `def klasifikasi(displacement):
    if displacement <= 2.0:
        return "Rendah"
    elif displacement <= 5.0:
        return "Sedang"
    else:
        return "Tinggi"

# Menerapkan fungsi ke dataframe untuk membuat kolom kelas baru
df["kelas"] = df["Perpindahan (mm)"].apply(klasifikasi)

# 2.1 Menentukan Kolom Fitur (X) dan Target (y)
fitur = ["Percepatan dasar (g)", "Percepatan struktur (g)"]
X = df[fitur]
y = df["kelas"]

print("Preprocessing dan pelabelan selesai!")
display(df.tail(12)) # Menampilkan beberapa data terakhir beserta kelasnya `
  
* Tahap pembersihan dan penerjemahan data angka mentah perpindahan bangunan menggunakan fungsi berbasis aturan `if-elif-else` untuk memberikan label kelas target (`Gempa Rendah`, `Gempa Sedang`, `Gempa Tinggi`), serta memisahkan variabel Fitur (X) dan Target (y).


4.  Model Training (Decision Tree Classifier):

* `from sklearn.tree import DecisionTreeClassifier

#3 Inisialisasi model Decision Tree dengan kriteria Gini
model = DecisionTreeClassifier(
    criterion="gini",
    max_depth=3,
    random_state=42
)

#3.1 Melatih model dengan data fitur (X) dan target (y)
model.fit(X, y)

print("Model Decision Tree berhasil dilatih!")`

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

* `import matplotlib.pyplot as plt
from sklearn.tree import plot_tree

#4 Membuat visualisasi pohon keputusan
plt.figure(figsize=(10, 6))
plot_tree(
    model, 
    feature_names=fitur, 
    class_names=model.classes_, 
    filled=True, 
    rounded=True,
    fontsize=10
)
plt.title("Visualisasi Pohon Keputusan Deteksi Gempa MPU6050", fontsize=12)
plt.show() `
  
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

* `# Input manual data dari simulasi sensor MPU
print("\n=== SIMULASI INPUT DATA SENSOR MPU ===")
acc_dasar = float(input("Masukkan Percepatan Dasar (g): "))
acc_struktur = float(input("Masukkan Percepatan Struktur (g): "))

# Validasi input tidak boleh negatif
if acc_dasar < 0 or acc_struktur < 0:
    raise ValueError("Nilai percepatan tidak boleh negatif!")

# Membungkus input ke dalam DataFrame untuk diprediksi model
data_baru = pd.DataFrame(
    [[acc_dasar, acc_struktur]],
    columns=fitur
)

# Melakukan prediksi kelas
hasil_prediksi = model.predict(data_baru)[0]

# Menampilkan hasil akhir
print("\n===== HASIL PREDIKSI STATUS GEMPA =====")
print(f"Percepatan Dasar    : {acc_dasar} g")
print(f"Percepatan Struktur : {acc_struktur} g")
print(f"Klasifikasi Status  : {hasil_prediksi.upper()}") `
  
* Tahap pengujian akhir di mana pengguna dapat memasukkan nilai input percepatan sensor secara interaktif untuk langsung mendapatkan hasil prediksi kategori status gempa secara *real-time*.
