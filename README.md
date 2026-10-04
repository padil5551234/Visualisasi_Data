# Paradoks Sumbagsel

Dasbor visualisasi interaktif tentang **kesejahteraan, struktur umur penduduk, dan jejaring migrasi antarprovinsi** di Sumatera bagian selatan (Jambi, Sumatera Selatan, Bengkulu, Lampung, Kepulauan Bangka Belitung). Seluruh data utama bersumber dari **Badan Pusat Statistik (BPS)**.

- **Tautan dasbor (publik, tanpa login):** https://padil5551234.github.io/Visualisasi_Data 
- **Repositori:** https://github.com/padil5551234/Visualisasi_Data 
- **Kode pengolahan data:** `scripts/olah_data.py`

> Proyek ini dibuat untuk tugas visualisasi data. Penulis: [Nama Penulis], [Program Studi], [Institusi].

## Pertanyaan yang dijawab

1. Di antara 60 kabupaten/kota Sumbagsel, wilayah mana yang IPM-nya tinggi tetapi kemiskinannya juga tinggi?
2. Seberapa berat beban penduduk nonproduktif (anak dan lansia) di tiap provinsi dan kabupaten/kota?
3. Provinsi mana yang menjadi pusat arus migrasi, dan apakah Sumbagsel menerima atau kehilangan penduduk?

## Tiga topik visualisasi

| Topik | Panel | Teknik |
|---|---|---|
| Data berdimensi tinggi (multivariat) | (a)–(e) | Parallel coordinates, PCA biplot dengan brushing, heatmap terklaster, kartu profil |
| Data berhierarki | (f)–(h) | Treemap dan sunburst dengan drill-down dan breadcrumb, batang beban umur |
| Data berjaring (network) | (i)–(j) | Node-link berarah-gaya dan matriks asal–tujuan |

Syarat minimal tiap topik terpenuhi: multivariat (8 variabel, 60 unit, PCA ditambah dua teknik lain, brushing, interpretasi kelompok dan pencilan); hierarki (3 jenjang di bawah akar, 2 representasi, drill-down); jejaring (34 simpul, 968 arus berbobot, metrik, komunitas, highlight tetangga, filter bobot).

## Interaksi

- **Klik** garis, titik, baris heatmap, blok treemap, atau simpul jejaring untuk memilih wilayah atau provinsi. Panel lain ikut menyorotnya.
- **Seret kotak** pada PCA untuk memilih beberapa wilayah (brushing). Garis dan heatmap ikut menyorot.
- **Drill-down** treemap dan sunburst: provinsi → kabupaten/kota → kelompok umur. Naik lewat breadcrumb atau lingkaran tengah sunburst.
- **Slider** "Tampilkan arus minimal" menyaring arus migrasi. Pilihan ukuran simpul: total arus atau betweenness.
- **Tooltip** pada setiap elemen. Pada layar sentuh, tooltip tampil saat diketuk.
- **Tombol** "Sorot anomali" dan pilihan 2 atau 3 kelompok.

## Sumber data

Semua data diakses pada **2 Oktober 2026**.

| Pilar | Sumber BPS | URL |
|---|---|---|
| Multivariat | *Provinsi Bengkulu dalam Angka 2025* | https://bengkulu.bps.go.id/id/publication/2025/02/28/655a1b91f093e3a2c2d6615f/provinsi-bengkulu-dalam-angka-2025.html |
| | *Provinsi Sumatera Selatan dalam Angka 2025* | https://sumsel.bps.go.id/id/publication/2025/02/28/6e08ccf1b88eba0383051826/provinsi-sumatera-selatan-dalam-angka-2025.html |
| | *Provinsi Lampung dalam Angka 2025* | https://lampung.bps.go.id/id/publication/2025/02/28/44f961867578243b5023c32d/provinsi-lampung-dalam-angka-2025.html |
| | *Provinsi Jambi dalam Angka 2025* | https://jambi.bps.go.id/id/publication/2025/02/28/7bfd5dfce1a5976105b05cd4/provinsi-jambi-dalam-angka-2025.html |
| | *Kepulauan Bangka Belitung Province in Figures 2025* | https://babel.bps.go.id/id/publication/2025/02/28/552f2d09ab64e68fd538b737/kepulauan-bangka-belitung-province-in-figures-2025.html |
| Hierarki | *Jumlah Penduduk menurut Kabupaten/Kota dan Kelompok Umur, 2025* (dari publikasi SUPAS 2025) | https://www.bps.go.id/id/publication/2026/06/30/bdc814f38edf0a941f4f9d34/penduduk-dan-indikator-kependudukan-hasil-survei-penduduk-antar-sensus-2025.html |
| Jejaring | *Penduduk dan Indikator Kependudukan Hasil SUPAS 2025*, migrasi risen antarprovinsi (angka = cacah sampel) | https://www.bps.go.id/id/publication/2026/06/30/bdc814f38edf0a941f4f9d34/penduduk-dan-indikator-kependudukan-hasil-survei-penduduk-antar-sensus-2025.html |

Tidak ada data non-BPS yang dipakai. Setiap panel di dasbor memuat baris "Sumber: BPS" dan bagian **Sumber Data** di bagian bawah halaman memuat daftar lengkap.

## Indikator kesejahteraan (pilar multivariat)

| Kode | Indikator | Satuan | Arah baik |
|---|---|---|---|
| IPM | Indeks Pembangunan Manusia | indeks | tinggi |
| RLS | Rata-rata lama sekolah | tahun | tinggi |
| Peng. | Pengeluaran per kapita | ribu Rp/tahun *(cek dengan publikasi)* | tinggi |
| PDRB | PDRB per kapita | Rp juta | tinggi |
| LPE | Laju pertumbuhan ekonomi | % | tinggi |
| Miskin | Persentase penduduk miskin | % | rendah |
| P1 | Indeks kedalaman kemiskinan | indeks | rendah |
| TPT | Tingkat pengangguran terbuka | % | rendah |

## Pengolahan data

Data lima publikasi provinsi digabung menjadi satu tabel 60 baris × 8 variabel (`data/indikator_kabkota.csv`). Seluruh langkah berikut dapat dijalankan ulang dengan `scripts/olah_data.py` (lihat bagian *Menjalankan ulang pengolahan data*):

- Setiap indikator dibakukan (skor-z). PCA dihitung pada matriks korelasi (PC1 39,1% dan PC2 25,4% varians).
- Wilayah dikelompokkan untuk k = 2 dan k = 3. Mutu kelompok dinilai dengan silhouette dan WCSS. Silhouette tertinggi ada pada k = 2 (0,39), sedangkan k = 3 hanya 0,27, sehingga kelompok dibaca sebagai tipologi, bukan pemisahan alami.
- Rasio ketergantungan = (penduduk di bawah 15 tahun + penduduk 65 tahun ke atas) ÷ penduduk 15–64 tahun × 100.
- Migrasi disusun sebagai matriks asal–tujuan 34 × 34 (968 arus). Dari sini dihitung migrasi neto (masuk − keluar), total arus, betweenness, dan komunitas simpul.
- Hasil olahan (`data/hasil_olahan.json`) ditanam langsung di `index.html` (objek `D`), sehingga dasbor tidak memanggil API apa pun.

## Alasan pilihan encoding visual

| Tugas | Encoding | Alasan |
|---|---|---|
| Membandingkan 8 indikator antarwilayah | Posisi pada sumbu sejajar | Posisi pada skala sama paling akurat dibaca (Cleveland dan McGill, 1984) |
| Mencari wilayah mirip | Posisi (PC1, PC2), bentuk = kelompok, ukuran = pencilan | Posisi untuk kemiripan, bentuk agar tidak bergantung pada warna |
| Membaca pola per kelompok | Warna divergen biru–jingga pada skor-z | Skor-z punya titik tengah bermakna (rata-rata 60 wilayah) |
| Membandingkan penduduk dan beban umur | Luas = penduduk, warna sekuensial = rasio ketergantungan | Dua variabel berbeda pada dua saluran visual berbeda |
| Mengenali pusat arus migrasi | Ukuran = total arus (atau betweenness), warna = komunitas, bentuk = surplus/defisit | Bentuk menambah informasi tanpa bergantung pada warna |
| Mencari koridor tertentu | Matriks berwarna sekuensial, diurut per komunitas | Matriks tidak punya tumpang tindih pada graf padat (Ghoniem dkk., 2004) |

Dua representasi per topik dipakai agar kelemahan satu representasi ditutup oleh yang lain: node-link bagus untuk pola umum tetapi padat, matriks bagus untuk graf padat.

## Menjalankan ulang pengolahan data

```bash
pip install -r requirements.txt
python scripts/olah_data.py        # membaca data/*.csv, menulis data/hasil_olahan.json
python scripts/verifikasi.py       # membandingkan dengan data yang tertanam di index.html
```

Keputusan pengolahan yang perlu diketahui:

| Langkah | Metode |
|---|---|
| Skor-z | `(x − rata-rata) / simpangan baku`, simpangan baku sampel (n − 1) |
| PCA | Dekomposisi nilai eigen pada matriks korelasi. Tanda komponen diatur agar IPM bermuatan positif pada PC1 |
| Klaster | k-means pada skor-z, `n_init=100`, `random_state=0`. Klaster dinamai A, B, C menurut rata-rata IPM menurun |
| Pencilan | Jarak Euclid ke pusat bidang (PC1, PC2) di atas 4,0. Ambang ini dipilih karena menghasilkan lima wilayah yang ditandai di dasbor; jika Anda memakai aturan lain, ubah `AMBANG_PENCILAN` |
| Rasio ketergantungan | `(muda + lansia) / (total − muda − lansia) × 100` |
| Betweenness | Graf tak-berarah, bobot pasangan = maksimum dari dua arah arus, jarak = 1 / bobot, `networkx.betweenness_centrality(normalized=True)` |
| Komunitas | `greedy_modularity_communities` pada graf tak-berarah dengan bobot = jumlah dua arah. Komunitas diberi nomor menurut total bobot menurun |
| Migrasi masuk/keluar | Total resmi dari tabel BPS (`migrasi_total_provinsi.csv`), bukan jumlah dari daftar arus, karena daftar arus hanya memuat sel bercacah ≥ 50 |

Hasil `verifikasi.py` pada data di repositori ini: PCA, klaster k = 2 dan 3, pencilan, silhouette dan WCSS untuk k = 2 sampai 5, betweenness, dan komunitas semuanya sama dengan dasbor. Satu selisih kecil ada pada k = 6 (silhouette 0,195 berbanding 0,194 di dasbor), yang tidak dipakai pada tampilan utama. Diuji dengan scikit-learn 1.8 dan networkx 3.6; versi lain dapat memberi selisih kecil pada k-means.

## Aksesibilitas dan perangkat

- **Palet ramah buta warna:** Okabe–Ito untuk provinsi dan komunitas, serta palet divergen biru–jingga. Informasi penting juga dikodekan dengan bentuk dan teks.
- **Huruf:** Atkinson Hyperlegible (Google Fonts), dengan cadangan `system-ui` bila gagal dimuat.
- **Responsif:** CSS Grid dengan titik henti 1000, 860, dan 700 px. Diuji pada Chromium *headless* dengan lebar 390, 768, 1100, dan 1280 px: tidak ada galat JavaScript dan tidak ada gulir horizontal pada halaman.
- **Sentuhan:** interaksi memakai *pointer events*, bukan hanya *mouse events*.
- **Gerak:** animasi dimatikan bila pengguna memilih *reduced motion*.
- Setiap grafik diberi label ARIA.

## Struktur repositori

```
.
├── index.html                      # dasbor (satu berkas, D3 v7.8.5 sudah disematkan)
├── data/
│   ├── indikator_kabkota.csv       # 8 indikator × 60 kab/kota (Provinsi dalam Angka 2025)
│   ├── penduduk_umur_kabkota.csv   # penduduk total, muda (0–14), lansia (65+) per kab/kota
│   ├── migrasi_antarprovinsi.csv   # arus asal → tujuan (cacah sampel, sel ≥ 50)
│   ├── migrasi_total_provinsi.csv  # total masuk dan keluar tiap provinsi (SUPAS 2025)
│   └── hasil_olahan.json           # keluaran olah_data.py, sama dengan objek D di index.html
├── scripts/
│   ├── olah_data.py                # pengolahan: skor-z, PCA, k-means, rasio, betweenness, komunitas
│   └── verifikasi.py               # membandingkan hasil_olahan.json dengan data di index.html
├── requirements.txt
├── docs/
│   └── Makalah_Paradoks_Sumbagsel_IEEE_A4.pdf   # opsional
└── README.md
```

> Ganti nama `Paradoks_Sumbagsel.html` menjadi `index.html` supaya GitHub Pages langsung menampilkannya.

## Menjalankan secara lokal

Tidak perlu instalasi. Cukup buka `index.html` di browser. Atau lewat server lokal:

```bash
python3 -m http.server 8000
# lalu buka http://localhost:8000
```

## Deployment (GitHub Pages)

1. Unggah berkas ke repositori publik di GitHub.
2. Buka **Settings → Pages**.
3. Pada *Source*, pilih **Deploy from a branch**, lalu cabang `main` dan folder `/ (root)`.
4. Simpan. Setelah beberapa menit, laman tersedia di `https://[username].github.io/[nama-repo]/`.
5. Tulis URL itu di bagian atas README ini dan di makalah.

Dasbor adalah laman statis tanpa server, sehingga juga dapat dihosting di Netlify, Vercel, atau layanan statis lain.

## Keterbatasan

- Data lintas-seksi (satu titik waktu), sehingga tren tidak terlihat.
- Migrasi berupa cacah sampel dan hanya tingkat provinsi. Ia tidak menjelaskan asal–tujuan pada tingkat kabupaten/kota, dan tidak boleh dibaca sebagai sebab-akibat bagi kesejahteraan.
- Struktur kelompok lemah (silhouette di bawah 0,5).
- Indikator berasal dari lima publikasi provinsi dan dapat berbeda dalam tahun referensi atau definisi. Penggabungannya dilakukan manual.
- Belum ada uji pengguna formal.
- Tidak ada peta, sehingga dimensi geospasial tidak tersaji.

## Teknologi

HTML, CSS, JavaScript, [D3.js](https://d3js.org) v7.8.5 (lisensi ISC, kode sumbernya disematkan di dalam `index.html`).

## Referensi utama

- M. Bostock, V. Ogievetsky, dan J. Heer, "D³: Data-driven documents," *IEEE Trans. Vis. Comput. Graphics*, vol. 17, no. 12, 2011.
- B. Shneiderman, "The eyes have it: A task by data type taxonomy for information visualizations," *Proc. IEEE Symp. Visual Languages*, 1996.
- M. Ghoniem, J.-D. Fekete, dan P. Castagliola, "A comparison of the readability of graphs using node-link and matrix-based representations," *Proc. IEEE InfoVis*, 2004.
- W. S. Cleveland dan R. McGill, "Graphical perception," *J. Amer. Statist. Assoc.*, vol. 79, no. 387, 1984.
- M. Okabe dan K. Ito, "Color universal design (CUD)," 2008. https://jfly.uni-koeln.de/color/

Daftar lengkap ada di makalah.

## Lisensi dan atribusi

- Kode dasbor: [pilih lisensi, mis. MIT].
- Data: © Badan Pusat Statistik. Gunakan sesuai ketentuan BPS dan cantumkan "Sumber: BPS".
