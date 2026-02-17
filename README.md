# Koreksi Bias Data CHIRPS: Metode Seasonal Scaling (Temporal Analysis)

![Python](https://img.shields.io/badge/Python-3.8%2B-blue)
![License](https://img.shields.io/badge/License-MIT-green)
![Status](https://img.shields.io/badge/Status-Research-orange)

## Deskripsi Proyek

Repositori ini memuat implementasi kode Python untuk melakukan **koreksi bias** pada data estimasi curah hujan satelit **CHIRPS v3.0**. Metode yang digunakan adalah **Seasonal Scaling** (Penskalaan Musiman/Temporal).

Pendekatan ini didasarkan pada asumsi bahwa bias satelit memiliki pola yang berbeda pada setiap musim (bulan). Oleh karena itu, metode ini menghitung **12 faktor koreksi unik** (satu untuk setiap bulan: Januari s.d. Desember) berdasarkan data historis jangka panjang.

## Metodologi

Koreksi dilakukan dengan mengalikan data satelit dengan *Seasonal Correction Factor* ($SF$) yang sesuai dengan bulan data tersebut.

Rumus koreksi adalah:

$$P_{corr}(m) = P_{sat}(m) \times SF_m$$

Dimana faktor koreksi untuk bulan ke-$m$ ($SF_m$) dihitung dengan:

$$SF_m = \frac{\sum P_{obs, m}}{\sum P_{sat, m}}$$

Keterangan:
- $m$: Indeks bulan (1 = Januari, ..., 12 = Desember).
- $\sum P_{obs, m}$: Total curah hujan observasi pada bulan $m$ selama periode studi.
- $\sum P_{sat, m}$: Total curah hujan satelit pada bulan $m$ selama periode studi.

Contoh: Faktor koreksi bulan **Januari** dihitung dari rata-rata seluruh data hujan bulan Januari (2009-2020). Faktor ini kemudian diterapkan pada setiap file raster bulan Januari.

## Prasyarat Instalasi

Script ini dikembangkan untuk lingkungan **Google Colab**. Berikut adalah dependensi pustaka yang digunakan:

### Daftar Pustaka
* **Geospasial:** `rasterio`, `geopandas`, `shapely`, `folium`
* **Analisis Data:** `pandas`, `numpy`, `scikit-learn`, `scipy`
* **Visualisasi:** `matplotlib`, `seaborn`
* **Utilitas:** `tqdm`

### Instalasi
```bash
pip install rasterio geopandas shapely matplotlib scikit-learn pandas numpy seaborn tqdm folium

```

## Struktur Direktori Data

Script ini membutuhkan struktur direktori data sebagai berikut di Google Drive:

```text
/Direktori_Project/
├── CHIRPS v3/                  # File raster .tif (input)
│   └── bias_corrected_temporal/ # (Output) Hasil koreksi
├── Batas Adm Jambi/            # Shapefile batas wilayah
├── Data Stasiun BWS/           # File .csv data curah hujan observasi
└── script_temporal.ipynb       # Script utama

```

## Cara Penggunaan

1. **Persiapan Data**: Pastikan data raster (CHIRPS) dan data stasiun (Time Series CSV) sudah siap.
2. **Setup Script**: Sesuaikan variabel `BASE_DIR`, `bias_stations` (kalibrasi), dan `validation_stations` (validasi).
3. **Jalankan Script**: Script akan bekerja dalam dua tahap:
* **Tahap Training**: Mengumpulkan data historis untuk menghitung 12 faktor koreksi ( sampai ).
* **Tahap Koreksi**: Menerapkan faktor tersebut ke seluruh file raster sesuai bulannya.


4. **Output**: Script menghasilkan file CSV validasi, grafik profil faktor koreksi bulanan, dan perbandingan statistik (NSE, R, RSR).

## Contoh Hasil Analisis

Metode ini sangat efektif untuk menangkap bias yang bersifat musiman (misal: CHIRPS cenderung *overestimate* saat musim hujan tetapi *underestimate* saat kemarau).

**Contoh Profil Faktor Koreksi:**

* **Januari (Musim Hujan):** Faktor = 0.85 (Satelit dikurangi karena overestimate)
* **Agustus (Kemarau):** Faktor = 1.20 (Satelit ditambah karena underestimate)

| Stasiun Validasi | NSE (Raw) | NSE (Corrected) | RSR (Raw) | RSR (Corrected) |
| --- | --- | --- | --- | --- |
| **Muara Tembesi** | -0.45 | **0.60** | 1.20 | **0.63** |
| **Rantau Pandan** | 0.12 | **0.55** | 0.93 | **0.68** |

## Penulis

**Jariyan Arifudin** Mahasiswa Geografi Lingkungan

Universitas Gadjah Mada (UGM)

## Lisensi & Sitasi

Kode ini didistribusikan di bawah **MIT License**.
Jika Anda menggunakan metode ini untuk penelitian, silakan sitasi repositori ini:

> Arifudin, J. (2026). *Koreksi Bias Data CHIRPS: Metode Seasonal Scaling (Temporal Analysis)*. GitHub Repository.
