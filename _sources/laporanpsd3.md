---
jupytext:
  formats: md:myst
  text_representation:
    extension: .md
    format_name: myst
    format_version: '0.13'
    jupytext_version: '1.11.5'
kernelspec:
  display_name: Python 3
  language: python
  name: python3
---
# Laporan Proyek Sains Data 3

<p align="center">
  <strong>LAPORAN PROYEK SAINS DATA</strong>
</p>

<p align="center">
  <strong>JUDUL</strong><br>
  (Harus singkat, jelas, dan langsung mencerminkan inti dari proyek)
</p>

![Logo Universitas Trunojoyo Madura](./utm.png)


<hr>

<p align="center">
  <strong>Disusun Oleh:</strong><br>
  Nama: Revika Syariqatun Alifia<br>
  NIM: 220411100008<br>
  Kelas: IF 5D
</p>

<p align="center">
  <strong>Dosen Pengampu:</strong><br>
  Nama: Mula'ab, S.Si., M.Kom<br>
  NIP: 19730520200212
</p>

<p align="center">
  <strong>PRODI TEKNIK INFORMATIKA</strong><br>
  <strong>JURUSAN TEKNIK INFORMATIKA</strong><br>
  <strong>FAKULTAS TEKNIK</strong><br>
  <strong>UNIVERSITAS TRUNOJOYO MADURA</strong><br>
  <strong>2024</strong>
</p>

<hr>

## PENDAHULUAN
<hr>

### Latar Belakang
<p style="text-indent: 20px;"> Gulaku didirikan pada tahun 2002 dengan tujuan menyediakan gula berkualitas tinggi yang terjangkau dan mudah didapat oleh masyarakat Indonesia. Setiap produk Gulaku diproduksi dengan standar kualitas yang konsisten, dikemas dalam kemasan steril untuk menjaga kebersihannya. Visi perusahaan ini adalah Memenuhi kebutuhan dan memuaskan konsumen dengan menyediakan gula yang terus ditingkatkan untuk mencapai keberlanjutan, sementara misinya adalah menyediakan produk berkualitas dan ramah lingkungan. Untuk menghadapi tantangan pasar yang dinamis, Gulaku menggunakan teknologi peramalan harga guna membantu produsen dan distributor dalam mengambil keputusan yang lebih tepat.</p>
<hr>
<p style="text-indent: 20px;"> Gulaku terus berkomitmen untuk meningkatkan keberlanjutan dalam setiap aspek operasionalnya. Dengan memanfaatkan teknologi canggih dalam proses produksinya, perusahaan tidak hanya memastikan kualitas tinggi, tetapi juga meminimalisir dampak lingkungan. Gulaku juga aktif berkolaborasi dengan pemerintah untuk memperkuat ekonomi lokal dan mendukung inisiatif sosial di Lampung. Melalui program pelatihan dan pengembangan karyawan, Gulaku berusaha menciptakan lingkungan kerja yang positif, di mana setiap karyawan dapat berkembang dan berkontribusi secara maksimal terhadap visi perusahaan. Dengan motto “Manis, Bersih, Murni, Alami,” Gulaku bertekad untuk memberikan yang terbaik bagi konsumennya. Namun, produsen dan distributor Gulaku menghadapi tantangan besar terkait perubahan harga akibat dinamika pasar dan ketidakpastian rantai pasokan.</p>
<hr>
<p style="text-indent: 20px;"> Untuk menghadapi tantangan harga yang sering berfluktuasi, Gulaku mengandalkan peramalan harga yang akurat. Dengan menganalisis data historis, perusahaan dapat mengurangi risiko perubahan harga yang tidak terduga, serta membantu produsen dan distributor dalam merencanakan produksi dan distribusi secara lebih efisien. Strategi ini membantu menjaga keseimbangan antara penawaran dan permintaan di pasar. Tujuan dari peramalan harga adalah memastikan stabilitas harga, menjaga ketersediaan produk yang konsisten, dan meminimalkan risiko kelebihan atau kekurangan stok.</p>
<hr>

### Tujuan
<p style="text-indent: 20px;"> Melalui analisis data historis, Gulaku dapat melakukan peramalan harga secara akurat untuk menjaga stabilitas harga di pasaran, memastikan ketersediaan produk yang konsisten, serta meminimalkan risiko kelebihan atau kekurangan stok. Dengan peramalan yang tepat, Gulaku dapat merencanakan produksi dan distribusi secara lebih efisien, serta mampu mempersiapkan diri terhadap perubahan cepat di pasar.</p>
<hr>

### Rumusan Masalah
<ul style="list-style-type: disc; padding-left: 20px; text-indent: 0;">
    <li>	Bagaimana Gulaku dapat memprediksi harga di pasar gula yang dinamis sehingga produsen dan distributor dapat mengambil keputusan yang lebih baik terkait produksi, distribusi, dan persediaan?</li>
</ul>
<hr>

## METODOLOGI

### Data Understanding

#### a. Sumber Data
Data yang digunakan pada proyek ini diambil dari platform PHIPS Nasional (Pusat Informasi Harga Pangan Strategis Nasional), yang beralamat di https://www.bi.go.id/hargapangan. Platform ini menyediakan informasi harga strategi pangan yang meliputi berbagai komoditas, baik di pasar tradisional maupun modern, serta data berdasarkan daerah dan jenis pedagang. Informasi ini mencakup harga rata-rata, perubahan harga, dan juga fitur visualisasi seperti histogram dan tampilan peta. Selain itu, PIHPS menawarkan berbagai jenis informasi harga pangan antar daerah, yang sangat berguna untuk analisis pasar dan pengambilan keputusan terkait kebutuhan pangan. Dalam proyek ini, digunakan data harga pangan yang tersedia dalam format tabel dari situs PIHPS Nasional, khususnya data harga gula dari tanggal 1 Januari 2021 hingga 18 September. Bentuk tampilan datanya seperti berikut :


```{code-cell} python
import pandas as pd
# Baca data CSV
df = pd.read_csv('https://raw.githubusercontent.com/chacalala/Dataset/refs/heads/main/datagula.csv')
print(df.head())
```

#### b. Deskripsi Dataset
Dataset ini terdiri dari 4 fitur atau kolom, dan 976 record. Berikut adalah deskripsi dari setiap atribut yang ada:

- **Tanggal** : Mengindentifikasi tanggal transaksi.
- **Bulan**: Mengindikasikan bulan terjadinya transaksi, di mana jumlah produk yang dibeli dalam satu transaksi dicatat.
- **Tahun**: Menunjukkan tahun terjadinya transaksi.
- **Harga Gula (kg)**: Menyimpan harga gula per kilogram, yang dapat berubah seiring waktu dan relevan dalam konteks pembelian.

melihat ringkasan tentang DataFrame
```{code-cell} python
df.info()
print('Ukuran data ', df.shape)
```
Hasilnya menunjukkan bahwa DataFrame memiliki 976 baris dan 4 kolom. Tiga kolom bertipe int64 (Tanggal, Bulan, Tahun) dan satu kolom bertipe object (Harga Gula). Semua kolom memiliki 976 nilai non-null, sehingga tidak ada nilai yang hilang. Penggunaan memori adalah sekitar 30.6 KB.

**Jenis Data**:
- **Tanggal** : Datetime
- **Bulan**: Kategorikal (Ordinal), karena bulan memiliki urutan tertentu (Januari, Februari, dst.).
- **Tahun**: Numerik (Diskrit), karena tahun adalah angka yang merepresentasikan waktu dan dapat dihitung.
- **Harga Gula (kg)**: Numerik (Kontinu), karena harga dapat memiliki nilai pecahan dan dapat diukur dengan presisi yang lebih tinggi.

melihat tipe data dari setiap kolom
```{code-cell} python
df.dtypes
```
Kolom Tanggal, Bulan, Tahun (int64) berisi bilangan bulat, sesuai karena merupakan angka. Harga Gula (kg) (object) bertipe teks, kemungkinan mengandung tanda koma, sehingga perlu dikonversi ke tipe numerik (seperti float) untuk perhitungan.

```{code-cell} python
print(df.describe())
```
memberikan informasi seperti jumlah data, rata-rata, nilai tertinggi dan terendah, serta seberapa jauh penyebaran data tersebut. Seperti misal :
- count : Jumlah total data untuk setiap kolom adalah 969.
- mean : Rata-rata nilai untuk setiap kolom, misalnya rata-rata untuk kolom xt adalah 15,260.11.
- std : Standar deviasi menunjukkan seberapa jauh nilai menyimpang dari rata-rata, dengan contoh kolom xtmemiliki 
- standar deviasi 1,567.58.
- min : Nilai minimum untuk setiap kolom, seperti nilai terkecil di kolom xtyang adalah 13,750.
- 25% (kuartil pertama) : 25% dari data memiliki nilai di bawah 13,950 untuk kolom xt-1.
- 50% (median) : Nilai tengah dari data, misalnya median untuk kolom xtadalah 14,800.
- 75% (kuartil ketiga) : 75% dari data memiliki nilai di bawah 15,550 untuk kolom xt-1.
- max : Nilai maksimum, di kolom mana xtmemiliki nilai tertinggi 18,950.

#### c. Eksplorasi Data
Sebelum memulai analisis atau visualisasi data, penting untuk memastikan bahwa kolom Harga Gula (kg) memiliki format yang benar agar bisa digunakan dalam perhitungan. Oleh karena itu, perlu mengubah kolom tersebut menjadi tipe data numerik dengan menghapus tanda koma yang mungkin ada.

```{code-cell} python
df['Harga Gula (kg)'] = pd.to_numeric(df['Harga Gula (kg)'].str.replace(',', ''), errors='coerce')
print(df)
```
membuat beberapa kolom baru di DataFrame untuk menyimpan harga gula berdasarkan lag waktu, yaitu harga 3, 2, 1 hari sebelumnya dan juga menambahkan kolom xt yang berisi harga gula saat ini. Setelah itu, hapus baris yang memiliki nilai kosong akibat proses shifting(menggeser nilai).

```{code-cell} python
df['xt-3'] = df['Harga Gula (kg)'].shift(-3)
df['xt-2'] = df['Harga Gula (kg)'].shift(-2)
df['xt-1'] = df['Harga Gula (kg)'].shift(-1) 
df['xt'] = df['Harga Gula (kg)']

df = df.dropna()
df.head()
```
Dengan menambahkan kolom yang menunjukkan harga gula 3, 2, dan 1 hari sebelumnya, kita bisa melihat bagaimana harga-harga tersebut mempengaruhi harga saat ini.

menambahkan kolom target xt (harga gula saat ini) ke DataFrame untuk diprediksi, dan menghapus kolom yang tidak diperlukan, sehingga hanya kolom penting untuk analisis yang tersisa.

```{code-cell} python
# Menyimpan harga gula saat ini ke kolom 'xt'
df['xt'] = df['Harga Gula (kg)']

# Menghapus baris dengan nilai NaN
df = df.dropna()

# Menghapus kolom 'Harga Gula (kg)' jika tidak diperlukan lagi
df = df.drop(columns=['Harga Gula (kg)']) 
```
menggabungkan kolom Bulan dan Tahun jadi date, lalu menghapus keduanya. Kolom date dijadikan indeks untuk memudahkan analisis berdasarkan tanggal.

```{code-cell} python
# Menggabungkan bulan dan tahun menjadi satu kolom tanggal
df['date'] = df['Bulan'].astype(str) + '-' + df['Tahun'].astype(str)
df.drop(columns=['Bulan', 'Tahun'], inplace=True)
df = df.set_index('date')

# Menampilkan DataFrame
print(df.head())
```
Kolom Bulan dan Tahun digabungkan menjadi kolom date untuk memudahkan analisis waktu. Menghapus kolom yang tidak diperlukan menyederhanakan DataFrame, dan menjadikan date sebagai indeks mempermudah operasi berbasis waktu.

Visualisasi ini dibuat untuk menunjukkan perubahan harga gula dari waktu ke waktu, termasuk harga 3, 2, dan 1 hari sebelumnya, serta harga saat ini. Dengan grafik garis, kita bisa melihat tren dan pola harga secara jelas. Ini membantu kita memahami bagaimana harga saat ini dipengaruhi oleh harga di hari-hari sebelumnya.

```{code-cell} python
import matplotlib.pyplot as plt
import seaborn as sns
# Visualisasi
plt.figure(figsize=(15, 5))
sns.lineplot(data=df[['xt-3', 'xt-2', 'xt-1', 'xt']])
plt.title('Harga Gula (xt-3, xt-2, xt-1, xt)')
plt.xlabel('Tanggal')
plt.ylabel('Harga Gula (kg)')
plt.xticks(rotation=90)
plt.tight_layout()
plt.show()
```
menghitung hubungan antara fitur-fitur lag (harga gula sebelumnya) dan harga gula saat ini. Dengan koefisien korelasi Pearson, kita bisa melihat seberapa kuat dan arah hubungan tersebut. Ini membantu kita memahami fitur mana yang paling berpengaruh terhadap harga gula saat ini, yang penting untuk analisis dan pengembangan model prediksi.

```{code-cell} python
import numpy as np
import pandas as pd
# Menghitung korelasi Pearson antara fitur dan target
correlations = {}
for col in df.columns[:-1]:  # Mengabaikan kolom terakhir (target)
    # Pastikan kolom adalah numerik
    correlation = np.corrcoef(df[col].astype(float), df['xt'].astype(float))[0, 1]
    correlations[col] = correlation

# Menampilkan hasil korelasi
for fitur, bobot_korelasi in correlations.items():
    print(f"Korelasi Pearson antara {fitur} dan target: {bobot_korelasi:.4f}")
```
dari hasil diatas
- Tanggal dan target (0.0163): Tidak ada hubungan signifikan; tanggal tidak mempengaruhi harga gula.
- xt-3 (0.9989): Hubungan sangat kuat; harga 3 hari lalu hampir sama dengan harga sekarang.
- xt-2 (0.9992): Hubungan sangat kuat; harga 2 hari lalu hampir identik dengan harga saat ini.
- xt-1 (0.9996): Hubungan sangat kuat; harga sehari lalu sangat mempengaruhi harga saat ini.
### Pra-pemrosesan Data

### Modelling

### Evaluation
