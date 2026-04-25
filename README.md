# EfficientNET-TTA-Data-Analyst-Deteksi-Anti-Spoofing



# Face Anti-Spoofing Classification Model

## Deskripsi

Project ini merupakan model computer vision yang dirancang untuk mendeteksi apakah gambar wajah berasal dari manusia asli atau merupakan percobaan penipuan (spoofing). Sistem seperti ini umum digunakan pada keamanan digital, verifikasi identitas, dan kompetisi machine learning berbasis citra.

Model dikembangkan dengan fokus pada performa tinggi namun tetap efisien. Pipeline ini dirancang agar memiliki akurasi yang baik, validasi yang stabil, tahan terhadap overfitting, hemat penggunaan GPU Kaggle T4, dan mampu bekerja pada dataset yang tidak seimbang.

---

## Jenis Kelas Prediksi

Model mampu mengenali enam kategori utama.

Kategori **realperson** digunakan untuk wajah manusia asli.  
Kategori **fake_printed** digunakan untuk serangan menggunakan foto cetak.  
Kategori **fake_screen** digunakan untuk serangan menggunakan layar ponsel, tablet, atau monitor.  
Kategori **fake_mask** digunakan untuk wajah palsu berupa masker.  
Kategori **fake_mannequin** digunakan untuk patung atau mannequin berbentuk wajah manusia.  
Kategori **fake_unknown** digunakan untuk jenis spoofing lain yang tidak termasuk kategori sebelumnya.

---

## Arsitektur Model

Model utama menggunakan **EfficientNet-B0** sebagai backbone klasifikasi gambar.

EfficientNet-B0 dipilih karena memiliki keseimbangan yang sangat baik antara akurasi dan efisiensi komputasi. Model ini cukup ringan, cepat dilatih, hemat memori GPU, dan sangat cocok digunakan pada lingkungan Kaggle GPU T4 maupun sistem dengan resource terbatas.

---

## Teknik Tambahan yang Digunakan

Selain backbone utama, model ini menggunakan beberapa teknik tambahan untuk meningkatkan performa.

### Texture Enhancement

Gambar asli tetap digunakan sebagai input utama, namun ditambahkan peningkatan tekstur ringan menggunakan edge detection. Teknik ini membantu model mengenali pola khas dari serangan spoofing seperti permukaan foto cetak, tekstur patung, atau detail tidak alami pada wajah palsu.

### EMA (Exponential Moving Average)

EMA digunakan untuk menyimpan versi bobot model yang lebih stabil selama proses training. Pendekatan ini sering membantu meningkatkan hasil validasi dan prediksi akhir.

### TTA (Test Time Augmentation)

Saat proses prediksi, gambar diuji dalam dua versi, yaitu gambar asli dan gambar hasil flip horizontal. Hasil prediksi keduanya kemudian dirata-ratakan agar lebih stabil dan akurat.

### Threshold Optimization

Jika confidence prediksi model terlalu rendah, hasil dapat dialihkan ke kelas **fake_unknown**. Teknik ini membantu menangani data baru yang sulit dikenali atau tidak sesuai pola training.

### Cross Validation

Model menggunakan **Stratified KFold Cross Validation** agar distribusi kelas pada data train dan valid tetap seimbang di setiap fold. Teknik ini membantu evaluasi model menjadi lebih adil dan terpercaya.

---

## Struktur Folder Dataset

Dataset sebaiknya disusun dalam struktur berikut:

```text
project/
│
├── train/
│   ├── realperson/
│   ├── fake_printed/
│   ├── fake_screen/
│   ├── fake_mask/
│   ├── fake_mannequin/
│   └── fake_unknown/
│
└── test/
