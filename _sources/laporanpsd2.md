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
# Laporan Proyek Sains Data 2

<p align="center">
  <strong>LAPORAN PROYEK SAINS DATA</strong>
</p>

<p align="center">
  <strong>Analisis Tren Historis untuk Prediksi Harga Gula</strong><br>
</p>

## PENDAHULUAN
<hr>

### Latar Belakang
<p style="text-indent: 20px;"> Gulaku, didirikan pada tahun 2002, berkomitmen menyediakan gula berkualitas tinggi yang terjangkau bagi masyarakat Indonesia. Dengan visi keberlanjutan dan misi ramah lingkungan, Gulaku menjaga kualitas produk melalui teknologi modern dan kemasan steril. Selain itu, perusahaan mendukung inisiatif sosial dan ekonomi lokal di Lampung.</p>
<hr>
<p style="text-indent: 20px;"> Menghadapi tantangan fluktuasi harga akibat dinamika pasar, Gulaku mengandalkan peramalan harga berbasis analisis data historis. Strategi ini membantu menjaga stabilitas harga, ketersediaan produk, serta mengurangi risiko kelebihan atau kekurangan stok. Peramalan yang akurat memungkinkan perusahaan merencanakan produksi dan distribusi lebih efisien untuk memenuhi kebutuhan pasar dengan lebih baik.</p>
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
df = pd.read_csv('https://raw.githubusercontent.com/chacalala/psd/refs/heads/main/Data%20Gula.csv')
print(df.head())
```

#### b. Deskripsi Dataset
Dataset ini terdiri dari 2 fitur atau kolom, dan 986 record. Berikut adalah deskripsi dari setiap atribut yang ada:

- **Date** : Mengindentifikasi tanggal transaksi, biasanya memiliki format YYYY-MM-DD.
- **Harga Gula (kg)**: Menyimpan harga gula per kilogram, yang dapat berubah seiring waktu dan relevan dalam konteks pembelian.

**Jenis Data**:
- **Date** : Data saat ini disajikan dalam bentuk string, namun akan diubah menjadi tipe data datetime pada tahap eksplorasi untuk memudahkan analisis waktu.
- **Harga Gula (kg)**: Numerik (Kontinu), karena harga dapat memiliki nilai pecahan dan dapat diukur dengan presisi yang lebih tinggi.

melihat tipe data dari setiap kolom
```{code-cell} python
df.dtypes
```
Kolom "Date" (object) berisi data dalam format teks, yang kemungkinan mewakili tanggal. Kolom "Harga" (object) bertipe teks, yang mungkin mengandung tanda koma atau simbol lainnya, sehingga perlu dikonversi ke tipe numerik (seperti float) untuk melakukan analisis.

#### c. Eksplorasi Data
Sebelum melakukan eksplorasi data, kolom date akan dikonversi dari format string menjadi tipe data datetime dan dijadikan sebagai indeks dari DataFrame.

```{code-cell} python
df['Date'] = pd.to_datetime(df['Date'], dayfirst=True, errors='coerce')
df.set_index('Date', inplace=True)
```

Selanjutnya, penting untuk memastikan bahwa kolom Harga Gula (kg) memiliki format yang benar agar bisa digunakan dalam perhitungan. Oleh karena itu, perlu mengubah kolom tersebut menjadi tipe data numerik dengan menghapus tanda koma yang mungkin ada.

```{code-cell} python
import numpy as np
df['Harga'] = df['Harga'].replace('-', np.nan).str.replace(',', '').astype(float)
print(df.head())
```

Setelah itu, melihat apakah didalam data set tersebut ada missing value

```{code-cell} python
missing_values = df.isnull().sum()
print("Jumlah Missing Values di setiap kolom:")
print(missing_values)
```

Setelah dilakukan pengecekan, ternyata terdapat missing values pada kolom Harga sebanyak 9 nilai. Untuk menangani hal ini, kita akan menggunakan metode Forward Fill (ffill), di mana nilai terakhir yang valid akan dibawa ke depan untuk mengisi nilai yang hilang. Metode ini dipilih karena dianggap cocok untuk mempertahankan kontinuitas data berdasarkan tren historis.

```{code-cell} python
df = df.ffill()
missing_values_after = df.isnull().sum()
print("\nJumlah Missing Values setelah penanganan:")
print(missing_values_after)
```

Selanjutnya melakukan pengecekan struktur dataset

```{code-cell} python
df.info()
print('Ukuran data ', df.shape)
```
DataFrame diatas berisi 986 data dengan indeks waktu dari 1 Januari 2021 hingga 11 Oktober 2024. Terdapat satu kolom bernama "Harga" bertipe float64, dan semua datanya lengkap tanpa nilai kosong. Ukuran memori yang digunakan adalah 15,4 KB. Dataset ini siap digunakan untuk analisis, seperti memprediksi harga atau melihat tren waktu.

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
