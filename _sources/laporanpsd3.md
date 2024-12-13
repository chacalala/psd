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

# Prediksi Harga Monero untuk Multi-Step Forecasting Berdasarkan Variabel Multivariat
## PENDAHULUAN
<hr>

### Latar Belakang
<p style="text-indent: 20px;"> Monero (XMR) adalah cryptocurrency yang terdesentralisasi dan berfokus pada privasi, pertama kali dirilis pada 2014 sebagai fork dari protokol CryptoNote. Berbeda dengan Bitcoin, Monero dirancang untuk menyediakan anonimitas penuh dalam transaksi, menggunakan teknologi seperti tanda tangan cincin (ring signatures), alamat sekali pakai (stealth addresses), dan mekanisme RingCT (Ring Confidential Transactions) untuk menyembunyikan identitas pengirim, penerima, serta jumlah transaksi. Hal ini menjadikan Monero pilihan populer di kalangan pengguna yang menghargai privasi tinggi, meskipun juga sering mendapat sorotan negatif dari regulator karena potensinya digunakan dalam aktivitas ilegal.</p>
<hr>
<p style="text-indent: 20px;"> Dalam dunia cryptocurrency, Monero (XMR) menonjol sebagai aset digital yang mengutamakan privasi dan keamanan. Namun, sifat pasar cryptocurrency yang sangat fluktuatif sering kali membuat investor dan pelaku pasar menghadapi tantangan besar dalam mengelola risiko dan memanfaatkan peluang investasi secara optimal. Monero memiliki potensi besar sebagai aset investasi alternatif, terutama bagi individu dan institusi yang menghargai privasi tinggi dalam transaksi keuangan. Akan tetapi, fluktuasi harga yang signifikan menciptakan ketidakpastian, yang pada gilirannya dapat menghambat adopsi lebih luas dan keputusan investasi yang tepat.</p>
<hr>
<p style="text-indent: 20px;"> Oleh karena itu, pengembangan model prediksi harga Monero menjadi sangat relevan untuk membantu investor memahami dinamika harga Monero secara lebih mendalam, mendukung strategi investasi yang lebih terinformasi dan berbasis data, serta meningkatkan kepercayaan terhadap aset ini dengan menyediakan wawasan yang dapat diandalkan terkait potensi pergerakan harga. Proyek ini dirancang untuk menjawab kebutuhan tersebut, dengan fokus pada pemanfaatan data historis dan teknologi kecerdasan buatan untuk memberikan solusi praktis bagi tantangan yang dihadapi oleh investor di pasar cryptocurrency.</p>
<hr>

### Tujuan
<p style="text-indent: 20px;"> Proyek ini bertujuan untuk mengembangkan model prediksi harga cryptocurrency Monero (XMR) berdasarkan data historis. Analisis ini diharapkan dapat membantu investor memahami pergerakan harga Monero, memaksimalkan keuntungan, dan mengelola risiko dengan lebih efektif.</p>
<hr>

### Rumusan Masalah
<ul style="list-style-type: disc; padding-left: 20px; text-indent: 0;">
    <li> Bagaimana mengembangkan model prediksi harga Monero (XMR) yang dapat diandalkan dengan menggunakan data historis, dan bagaimana hasil prediksi tersebut dapat mendukung pengambilan keputusan investasi yang lebih strategis di pasar cryptocurrency?</li>
</ul>
<hr>

## METODOLOGI

### Data Understanding

#### a. Sumber Data
Data yang digunakan pada proyek ini diambil dari platform Yahoo Finance, yang beralamat di https://finance.yahoo.com/quote/XMR-USD/. Platform ini menyediakan informasi harga Monero (XMR) yang mencakup data historis harga Monero terhadap dolar AS (USD) dari berbagai periode waktu. Informasi ini termasuk harga penutupan, perubahan harga harian, dan berbagai fitur analisis pasar. Dalam proyek ini, digunakan data harga Monero yang tersedia dalam format CSV, mencakup periode dari tanggal 2020 Januari 01 hingga 2024 Desember 06. Bentuk tampilan datanya seperti berikut :


```{code-cell} python
import pandas as pd
# Baca data CSV
df = pd.read_csv('https://raw.githubusercontent.com/chacalala/psd/refs/heads/main/monero.csv')
print(df.head())
```

<p style="text-indent: 50px; text-align: justify;">Di sini, perubahan dilakukan agar kolom Date bisa diproses dengan benar sebagai tanggal. Dengan mengubahnya ke format datetime, kita bisa lebih mudah melakukan perbandingan atau analisis berdasarkan waktu. Kemudian, menjadikan Date sebagai indeks membuat data lebih mudah dicari berdasarkan tanggal, dan menyortirnya memastikan data tersusun dengan urutan waktu yang benar.</p>

```{code-cell} python
# mengubah kolom 'Date' dalam format datetime
df['Date'] = pd.to_datetime(df['Date'])

# Mengatur kolom 'Date' sebagai indeks
df.set_index('Date', inplace=True)

# Mensortir data berdasarkan kolom Date dari terkecil ke terbesar
df = df.sort_values(by='Date')
df
```

#### b. Deskripsi Dataset
<p style="text-indent: 50px; text-align: justify;">Dataset ini terdiri dari 8 fitur atau kolom, dan 2230 record atau baris. Berikut adalah penjelasan setiap atribut:</p>
        <ul>
            <li><strong>Date:</strong> Tanggal data harga aset koin (format YYYY-MM-DD)</li>
            <li><strong>Open:</strong> Harga pembukaan aset koin pada tanggal tersebut</li>
            <li><strong>High:</strong> Harga tertinggi yang dicapai pada tanggal tersebut</li>
            <li><strong>Low:</strong> Harga terendah aset koin pada tanggal tersebut</li>
            <li><strong>Close:</strong> Harga penutupan aset koin pada tanggal tersebut</li>
            <li><strong>Adj Close:</strong> Harga penutupan yang sudah disesuaikan dengan pembagian aset koin, dividen, dan corporate actions lainnya</li>
            <li><strong>Volume:</strong> Jumlah aset koin yang diperdagangkan pada tanggal tersebut</li>
        </ul>

<p style="text-indent: 50px; text-align: justify;"> Selanjutnya, memeriksa informasi tentang dataset untuk melihat jumlah baris dan kolom, serta tipe data dari masing-masing kolom. Ini membantu kita memastikan bahwa semua data sudah lengkap dan tidak ada yang hilang. Selain itu, kita juga ingin mengetahui ukuran data untuk melihat berapa banyak baris dan kolom yang ada. Dari hasil ini, kita bisa tahu bahwa dataset memiliki 1802 baris dan 6 kolom, dengan data yang lengkap dan tipe data yang sesuai.</p>

```{code-cell} python
df.info()
print('Ukuran data ', df.shape)
```

#### c. Eksplorasi Data

<p style="text-indent: 50px; text-align: justify;">Setelah itu, melihat apakah didalam data set tersebut ada missing value.</p>

```{code-cell} python
df.isnull().sum()
```

<p style="text-indent: 50px; text-align: justify;">Setelah dilakukan pengecekan dan ternyata tidak ada missing value maka langkah selanjutnya yaitu membuat visualisasi tren data untuk setiap kolom dalam dataset. Kita akan menggambar grafik garis untuk setiap kolom, dengan tanggal sebagai sumbu X dan nilai kolom sebagai sumbu Y. Setiap grafik akan menampilkan tren perubahan nilai kolom tersebut seiring waktu, lengkap dengan label sumbu dan judul yang menjelaskan kolom yang sedang dianalisis. Grafik ini juga akan dilengkapi dengan grid dan rotasi sumbu X agar lebih mudah dibaca.</p>

```{code-cell} python
import matplotlib.pyplot as plt
import seaborn as sns
# Membuat subplot otomatis berdasarkan jumlah kolom dalam dataframe
plt.figure(figsize=(9, int(np.ceil(len(df.columns) / 3)) * 3))

for i, col in enumerate(df.columns):
    plt.subplot(int(np.ceil(len(df.columns) / 3)), 3, i + 1)
    sns.lineplot(data=df, x='Date', y=col)
    plt.title(f'Trend of {col}')
    plt.xticks(rotation=45)

plt.tight_layout()
plt.show()
```

<p style="text-indent: 50px; text-align: justify;">Selanjutnya membuat histogram dari kolom Open, High, Low, Close, Adj Close, dan Volume bertujuan untuk memvisualisasikan distribusi data pada setiap kolom tersebut. Visualisasi ini membantu kita memahami pola data, seperti apakah data terdistribusi normal, memiliki outlier, atau cenderung miring (skewed). Dengan histogram, kita dapat mengevaluasi karakteristik data sebelum melanjutkan ke tahap analisis atau pemodelan selanjutnya.</p>

```{code-cell} python
df.hist(figsize=(10, 8), column=['Open', 'High', 'Low', 'Close', 'Adj Close', 'Volume'])
```

<p style="text-indent: 50px; text-align: justify;">Setelah itu, di sini kita akan menganalisis hubungan antar fitur dalam dataset. Heatmap ini membantu kita memahami korelasi antara kolom-kolom yang ada, apakah ada hubungan yang kuat, lemah, atau tidak ada korelasi sama sekali. Ini dapat memberikan wawasan penting untuk analisis lebih lanjut atau pembuatan model, seperti memilih fitur yang relevan atau mendeteksi multikolinearitas.</p>

```{code-cell} python
correlation_matrix = df.corr()

plt.figure(figsize=(7, 3))
sns.heatmap(correlation_matrix, annot=True, cmap='coolwarm', linewidths=0.5)
plt.title('Heatmap Korelasi Antar Fitur')
plt.show()
```
<p style="text-indent: 50px; text-align: justify;">Hasil korelasi pada heatmap ini menunjukkan hubungan antara fitur-fitur dalam data. Nilai korelasi berkisar dari -1 sampai 1, di mana 1 berarti hubungan sangat kuat dan positif, 0 berarti tidak ada hubungan, dan -1 berarti hubungan sangat kuat namun negatif. Pada heatmap ini, terlihat bahwa fitur "Open," "High," "Low," "Close," dan "Adj Close" memiliki korelasi sempurna (1), yang menunjukkan bahwa nilai-nilai ini sangat saling terkait. Namun, fitur "Volume" memiliki korelasi yang lemah (sekitar 0,26-0,27) terhadap fitur-fitur lainnya, yang berarti perubahan pada "Volume" tidak terlalu berhubungan dengan perubahan pada fitur lainnya.</p>

### Data Preprocessing

#### a. Penghapusan Kolom yang Tidak Relevan dan Verifikasi Dataset
<p style="text-indent: 50px; text-align: justify;">Menghapus kolom Volume dan Adj Close dari dataset, kolom-kolom tersebut dihilangkan karena mungkin tidak relevan untuk analisis atau pemodelan selanjutnya. Kemudian, kita akan melihat lima baris pertama dari dataset yang telah diperbarui untuk memastikan perubahan yang dilakukan.</p>

```{code-cell} python
df = df.drop(columns=['Volume', 'Adj Close'])
df.head()
```

#### b. Rekayasa Fitur
<p style="text-indent: 50px; text-align: justify;">Dalam penelitian ini, tujuan kita adalah memprediksi harga **Close** pada hari berikutnya, sehingga kita memerlukan variabel baru sebagai target. Fitur ini berguna untuk mengetahui sejauh mana harga saham bisa turun, yang memungkinkan investor untuk membeli aset pada harga yang lebih rendah dan meningkatkan peluang keuntungan ketika harga saham naik kembali.</P>

```{code-cell} python
df['Close Target'] = df['Close'].shift(-1)

df = df[:-1]
df.head()
```

<p style="text-indent: 50px; text-align: justify;">Tabel ini menunjukkan data harga saham harian yang mencakup harga pembukaan (Open), harga tertinggi (High), harga terendah (Low), dan harga penutupan (Close). Selain itu, terdapat kolom Close Target yang merupakan prediksi harga penutupan untuk hari berikutnya. Misalnya, pada 2020-01-01, harga penutupan adalah 7200.2, sementara prediksi harga penutupan untuk 2020-01-02 adalah 6985.5, yang lebih rendah dari harga penutupan hari itu.</P>


#### c. Normalisasi Data

<p style="text-indent: 50px; text-align: justify;">Melakukan normalisasi data pada fitur dan target bertujuan untuk mengubah nilai-nilai dalam dataset ke rentang yang seragam, biasanya antara 0 dan 1. Dalam kode ini, `MinMaxScaler` digunakan untuk menormalisasi fitur (Open, High, Low, Close) dan target (Close Target). Fitur dinormalisasi menggunakan scaler_features.fit_transform() dan target menggunakan scaler_target.fit_transform(). Hasil normalisasi kemudian digabungkan dengan pd.concat() menjadi satu dataframe df_normalized, yang siap digunakan dalam model machine learning. Normalisasi membantu model belajar lebih efektif dengan skala data yang konsisten.</p>

```{code-cell} python
# Inisialisasi scaler untuk fitur (input) dan target (output)
scaler_features = MinMaxScaler()
scaler_target = MinMaxScaler()

# Normalisasi fitur (Open, High, Low,, 'Close' Close Target-4, Close Target-5)
df_features_normalized = pd.DataFrame(scaler_features.fit_transform(df[['Open', 'High', 'Low', 'Close']]),
                                      columns=['Open', 'High', 'Low', 'Close'],
                                      index=df.index)

# Normalisasi target (Close Target)
df_target_normalized = pd.DataFrame(scaler_target.fit_transform(df[['Close Target']]),
                                    columns=['Close Target'],
                                    index=df.index)

# Gabungkan kembali dataframe yang sudah dinormalisasi
df_normalized = pd.concat([df_features_normalized, df_target_normalized], axis=1)
df_normalized.head()
```

#### Modelling
#### a. Pembagian Data 

<p style="text-indent: 50px; text-align: justify;">Selanjutnya, melakukan pembagian data menjadi data training dan data testing dengan menggunakan train_test_split, di mana 80% data digunakan untuk training dan 20% untuk testing. Proses ini dilakukan dengan opsi shuffle=False agar data tetap terurut berdasarkan urutan aslinya. Setelah pembagian, data training (X_train dan y_train) digunakan untuk melatih model, sedangkan data testing (X_test dan y_test) digunakan untuk menguji kinerja model yang telah dilatih.

```{code-cell} python
# Mengatur fitur (X) dan target (y)
X = df_normalized[['Open', 'High', 'Low', 'Close']]
y = df_normalized['Close Target']

# Membagi data menjadi training dan testing (60% training, 40% testing)
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.4, shuffle=False)
```

#### b. Penyusunan Model

<p style="text-indent: 50px; text-align: justify;">Pada tahap ini, dilakukan percobaan dengan menggunakan tiga model utama, yaitu Regresi Linear, Ridge Linear , dan Gradient Boosting. Selain itu, untuk meningkatkan akurasi dan kinerja model, diterapkan juga teknik ensemble dengan metode bagging</p>

```{code-cell} python
# List model regresi
models = {
    "Linear Regression": LinearRegression(),
    "Ridge Regression": Ridge(alpha=1.0),
    "Gradient Boosting" : GradientBoostingRegressor(random_state=32),
}

# Dictionary untuk menyimpan hasil evaluasi
results = {}

# Iterasi setiap model
for name, model in models.items():
    # Latih model
    model.fit(X_train, y_train)

    # Prediksi pada data uji
    y_pred = model.predict(X_test)

    # Evaluasi
    mse = mean_squared_error(y_test, y_pred)
    rmse = np.sqrt(mse)
    mape = mean_absolute_percentage_error(y_test, y_pred) * 100  # Dalam persen

    # Simpan hasil evaluasi
    results[name] = {"RMSE": rmse, "MAPE": mape}

    # Kembalikan hasil prediksi ke skala asli
    y_pred_original = scaler_target.inverse_transform(y_pred.reshape(-1, 1))
    y_test_original = scaler_target.inverse_transform(y_test.values.reshape(-1, 1))

    # Plot hasil prediksi
    plt.figure(figsize=(15, 6))
    plt.plot(y_test.index, y_test_original, label="Actual", color="blue")
    plt.plot(y_test.index, y_pred_original, label=f"Predicted ({name})", color="red")

    # Tambahkan detail plot
    plt.title(f'Actual vs Predicted Values ({name})')
    plt.xlabel('Date')
    plt.ylabel('Harga')
    plt.legend()
    plt.grid(True)

    # Tampilkan plot
    plt.show()

# Tampilkan hasil evaluasi
print("HASIL EVALUASI MODEL")
for model, metrics in results.items():
    print(f"{model}:\n  RMSE: {metrics['RMSE']:.2f}\n  MAPE: {metrics['MAPE']:.2f}%\n")
```

### Kesimpulan
<p style="text-indent: 50px; text-align: justify;">Berdasarkan hasil percobaan dengan beberapa model, metode Linear Regression menunjukkan performa terbaik dengan nilai RMSE sebesar 0.01 dan MAPE sebesar 2.01%.</P>

### DEPLOYMENT
Hasil deployment dapat dilihat melalui tautan berikut:
https://huggingface.co/spaces/Alifiacaca/projek1_prediksibtc