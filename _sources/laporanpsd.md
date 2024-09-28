# Laporan Proyek Sains Data

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
    <li>•	Bagaimana Gulaku dapat memprediksi harga di pasar gula yang dinamis sehingga produsen dan distributor dapat mengambil keputusan yang lebih baik terkait produksi, distribusi, dan persediaan?</li>
</ul>
<hr>

## METODOLOGI

### Data Understanding

#### a. Sumber Data
Data yang digunakan pada proyek ini diambil dari platform **Lamin Etam** (Laman Informasi Ekonomi Komoditas Kaltim), yang dapat diakses melalui tautan berikut: [Lamin Etam](https://hargapangan.laminetam.id/tabel-harga/daerah). Dataset ini mencakup informasi harga pangan di berbagai daerah di Indonesia, khususnya dari Samarinda dan Kalimantan Timur. **Lamin Etam** merupakan platform yang menyediakan informasi harga pangan serta menjadi showcase untuk menampilkan UMKM unggulan dari Samarinda dan Kalimantan Timur. Pada proyek ini, digunakan data harga pangan yang tersedia dalam format tabel dari situs tersebut.

#### b. Deskripsi Dataset
Dataset ini terdiri dari 3 fitur atau kolom, dengan total 56 record. Berikut adalah deskripsi dari setiap atribut yang ada:

- **Bulan**: Mengindikasikan bulan terjadinya transaksi, di mana jumlah produk yang dibeli dalam satu transaksi dicatat.
- **Tahun**: Menunjukkan tahun terjadinya transaksi.
- **Harga Gula (kg)**: Menyimpan harga gula per kilogram, yang dapat berubah seiring waktu dan relevan dalam konteks pembelian.

**Jenis Data**:
- **Bulan**: Kategorikal (Ordinal), karena bulan memiliki urutan tertentu (Januari, Februari, dst.).
- **Tahun**: Numerik (Diskrit), karena tahun adalah angka yang merepresentasikan waktu dan dapat dihitung.
- **Harga Gula (kg)**: Numerik (Kontinu), karena harga dapat memiliki nilai pecahan dan dapat diukur dengan presisi yang lebih tinggi.

### Pra-pemrosesan Data
Langkah-langkah pada tahap ini adalah sebagai berikut:

#### a. Menangani Nilai Hilang
Data yang hilang diidentifikasi dan dihapus untuk memastikan tidak ada celah dalam dataset.
```python\`\`\`
df = df.dropna()

#### b.	Konversi Tipe Data
Kolom Tahun dan Bulan diubah menjadi tipe integer untuk memudahkan pemodelan.
```python\`\`\`
df['Tahun'] = df['Tahun'].astype(int)
df['Bulan'] = df['Bulan'].astype(int)

#### c.	Pembersihan Variabel Target
Variabel target Harga Gula (kg) memiliki nilai dengan simbol mata uang 'Rp' dan koma. Ini dihapus, dan kolom diubah menjadi tipe float.
```python\`\`\`
df['Harga Gula (kg)'] = df['Harga Gula (kg)'].str.replace('Rp', '').str.replace(',', '').str.strip().astype(float)

#### d.	Deteksi dan Penghapusan Outlier
Z-score dihitung untuk mendeteksi outlier, dan baris dengan Z-score lebih dari 3 dianggap sebagai outlier dan dihapus.
```python\`\`\`
z_scores = np.abs(stats.zscore(df['Harga Gula (kg)'])) outliers = np.where(z_scores > threshold) df_no_outliers = df.drop(outliers[0])

### Modelling

#### a.	Mendefinisikan Fitur dan Target
Fitur yang digunakan adalah Bulan dan Tahun, sementara variabel target adalah Harga Gula (kg).
```python\`\`\`
X = df_no_outliers[['Bulan', 'Tahun']]
y = df_no_outliers['Harga Gula (kg)']

#### b.	Pembagian Data
Data dibagi menjadi data latih (80%) dan data uji (20%) menggunakan fungsi train_test_split dari scikit-learn.
```python\`\`\`
X = df_no_outliers[['Bulan', 'Tahun']]
y = df_no_outliers['Harga Gula (kg)']