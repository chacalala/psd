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
    <li>	Bagaimana Gulaku dapat memprediksi harga di pasar gula yang dinamis sehingga produsen dan distributor dapat mengambil keputusan yang lebih baik terkait produksi, distribusi, dan persediaan?</li>
</ul>
<hr>

## METODOLOGI

### Data Understanding

#### a. Sumber Data
Data yang digunakan pada proyek ini diambil dari platform PHIPS Nasional (Pusat Informasi Harga Pangan Strategis Nasional), yang beralamat di https://www.bi.go.id/hargapangan. Platform ini menyediakan informasi harga strategi pangan yang meliputi berbagai komoditas, baik di pasar tradisional maupun modern, serta data berdasarkan daerah dan jenis pedagang. Informasi ini mencakup harga rata-rata, perubahan harga, dan juga fitur visualisasi seperti histogram dan tampilan peta. Selain itu, PIHPS menawarkan berbagai jenis informasi harga pangan antar daerah, yang sangat berguna untuk analisis pasar dan pengambilan keputusan terkait kebutuhan pangan. Dalam proyek ini, digunakan data harga pangan yang tersedia dalam format tabel dari situs PIHPS Nasional, khususnya data harga gula dari tanggal 1 Januari 2021 hingga 18 September. Bentuk tampilan datanya seperti berikut :
```python
import pandas as pd
# Baca data CSV
df = pd.read_csv('https://raw.githubusercontent.com/chacalala/Dataset/refs/heads/main/datagula.csv')
print(df.head())

```

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

### Modelling

### Evaluation
