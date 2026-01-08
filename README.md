## Navyta Budi Yulia (312410184)
## TI.24.A3

# Penjelasan Code Analisa Data Zamato 


## Langkah 1: Mengimpor Pustaka Python yang Diperlukan

```
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
```
### Penjelasan :
- Pada tahap awal, dilakukan impor beberapa pustaka Python yang akan digunakan selama proses analisis data.
- Library Pandas digunakan untuk membaca, mengelola, dan memanipulasi dataset berbentuk tabel.
- Library NumPy digunakan untuk mendukung operasi numerik dan pengolahan data numerik.
- Library Matplotlib dan Seaborn digunakan untuk menampilkan visualisasi data dalam bentuk grafik, diagram batang, histogram, dan heatmap agar pola data dapat diamati dengan lebih mudah.

## Langkah 2: Membuat Bingkai Data (DataFrame)

```
dataframe = pd.read_csv("Zomato-data-.csv")
print(dataframe.head())
```
### Penjelasan :
- Dataset Zomato dibaca menggunakan fungsi `read_csv()` dan disimpan dalam sebuah objek DataFrame.
- DataFrame merupakan struktur data dua dimensi yang menyerupai tabel, sehingga memudahkan proses analisis.
- Fungsi `head()` digunakan untuk menampilkan lima baris pertama dataset sebagai langkah verifikasi awal bahwa data telah berhasil dimuat dan untuk memahami struktur kolom yang tersedia.

## Langkah 3: Pembersihan dan Persiapan Data
## 3.1 Konversi Kolom Rating Menjadi Float

```
def handleRate(value):
    value = str(value).split('/')
    value = value[0]
    return float(value)

dataframe['rate'] = dataframe['rate'].apply(handleRate)
print(dataframe.head())
```
### Penjelasan :
Kolom `rate` pada dataset awalnya memiliki format teks seperti `4.1/5`, sehingga tidak dapat langsung digunakan untuk analisis statistik.
Oleh karena itu, dibuat fungsi `handleRate()` untuk:

- Mengubah nilai menjadi string

- Memisahkan nilai rating dari karakter `/`

- Mengambil nilai rating saja

- Mengonversinya menjadi tipe data float

Fungsi ini kemudian diterapkan ke seluruh kolom `rate` menggunakan `apply()`.
Setelah proses pembersihan selesai, data ditampilkan kembali untuk memastikan konversi berhasil.

## 3.2 Menampilkan Informasi DataFrame

```
dataframe.info()
```

### Penjelasan :
Fungsi info() digunakan untuk menampilkan ringkasan informasi dataset, termasuk:

- Jumlah baris dan kolom

- Nama kolom

- Tipe data masing-masing kolom

- Jumlah data yang tidak kosong

Langkah ini penting untuk memastikan bahwa tipe data sudah sesuai dan dataset siap untuk dianalisis.

## 3.3 Memeriksa Nilai Kosong

```
print(dataframe.isnull().sum())
```

### Penjelasan :
- Kode ini digunakan untuk menghitung jumlah nilai kosong (null) pada setiap kolom.
- Pemeriksaan ini penting untuk mengidentifikasi adanya kesenjangan data yang dapat mempengaruhi hasil analisis.
- Berdasarkan hasil output, dataset tidak memiliki nilai kosong sehingga tidak diperlukan proses penghapusan atau pengisian data.

## Langkah 4: Menjelajahi Jenis Restoran
## 4.1 Distribusi Jenis Restoran

```
grouped_data = dataframe.groupby('listed_in(type)')['votes'].sum()
result = pd.DataFrame({'votes': grouped_data})

plt.plot(result, c='green', marker='o')
plt.xlabel('Type of restaurant')
plt.ylabel('Votes')
```

### Penjelasan :
- Data dikelompokkan berdasarkan jenis restoran menggunakan `groupby()`, kemudian dijumlahkan nilai voting-nya.
- Hasilnya ditampilkan dalam bentuk grafik garis untuk menunjukkan tingkat popularitas setiap jenis restoran berdasarkan jumlah vote yang diberikan oleh pelanggan.

## Langkah 5: Identifikasi Restoran yang Paling Banyak Dipilih

```
max_votes = dataframe['votes'].max()
restaurant_with_max_votes = dataframe.loc[
    dataframe['votes'] == max_votes, 'name'
]

print('Restaurant(s) with the maximum votes:')
print(restaurant_with_max_votes)
```
### Penjelasan :
- Kode ini digunakan untuk mencari nilai vote tertinggi dalam dataset dan mengidentifikasi restoran yang memiliki jumlah vote tersebut.
- Hasil output menunjukkan bahwa Empire Restaurant merupakan restoran dengan tingkat popularitas tertinggi.

## Langkah 6: Ketersediaan Pesanan Online

```
sns.countplot(x=dataframe['online_order'])
```

### Penjelasan :
- Grafik ini menampilkan perbandingan jumlah restoran yang menyediakan layanan pemesanan online dan yang tidak.
- Visualisasi ini membantu memahami sejauh mana layanan online diadopsi oleh restoran.

## Langkah 7: Analisis Peringkat

```
plt.hist(dataframe['rate'], bins=5)
plt.title('Ratings Distribution')
plt.show()
```
### Penjelalsan :
- Histogram digunakan untuk melihat distribusi nilai rating restoran.
- Pembagian ke dalam beberapa interval (bins) membantu mengamati rentang rating yang paling sering muncul.
- Hasil menunjukkan bahwa mayoritas restoran memiliki rating menengah hingga tinggi.

## Langkah 8: Perkiraan Biaya untuk Pasangan

```
couple_data = dataframe['approx_cost(for two people)']
sns.countplot(x=couple_data)
```

### Penjelasan :
- Analisis ini bertujuan untuk mengetahui kisaran biaya makan untuk dua orang yang paling diminati pelanggan.
- Grafik batang menunjukkan bahwa kisaran harga menengah paling sering muncul dalam dataset.

## Langkah 9: Perbandingan Peringkat Online dan Offline

```
plt.figure(figsize=(6,6))
sns.boxplot(x='online_order', y='rate', data=dataframe)
```

### Penjelasan :
- Boxplot digunakan untuk membandingkan distribusi rating antara restoran yang menerima pesanan online dan yang tidak.
- Grafik ini memperlihatkan perbedaan median dan sebaran rating, sehingga dapat diketahui kecenderungan kualitas layanan berdasarkan mode pemesanan.

```
pivot_table = dataframe.pivot_table(
    index='listed_in(type)',
    columns='online_order',
    aggfunc='size',
    fill_value=0
)

sns.heatmap(pivot_table, annot=True, cmap='YlGnBu', fmt='d')
plt.title('Heatmap')
plt.xlabel('Online Order')
plt.ylabel('Listed In (Type)')
plt.show()
```
### Penjelasan :
- Tabel pivot digunakan untuk menghitung jumlah restoran berdasarkan jenis restoran dan ketersediaan   layanan pemesanan online.
- Heatmap kemudian digunakan untuk memvisualisasikan hubungan tersebut secara lebih jelas.
- Hasil visualisasi menunjukkan bahwa kafe lebih dominan menerima pesanan online, sedangkan restoran lebih banyak melayani pelanggan secara langsung (offline).

