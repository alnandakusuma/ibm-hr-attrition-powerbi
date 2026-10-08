# Dashboard HR Attrition (Power BI)

Dashboard Power BI interaktif yang menelusuri **mengapa karyawan keluar dari perusahaan** dan karakteristik tenaga kerja apa saja yang berkaitan dengan tingkat attrition yang lebih tinggi. Proyek ini dikerjakan dari awal sampai akhir: pembersihan data di Power Query, pemodelan data dengan measure DAX, dan laporan tiga halaman lengkap dengan rekomendasi untuk HR.

> Catatan: label dan judul di dalam dashboard memakai Bahasa Inggris agar konsisten dengan data aslinya. Dokumentasi ini ditulis dalam Bahasa Indonesia.

## Tampilan Dashboard

**1. Overview (Ringkasan)**
![Overview](screenshots/01-overview.png)

**2. Drivers (Faktor Penyebab)**
![Drivers](screenshots/02-drivers.png)

**3. Insights & Recommendations (Temuan dan Rekomendasi)**
![Insights](screenshots/03-insights.png)

## Pertanyaan Bisnis

1. Berapa tingkat attrition keseluruhan, dan departemen serta jabatan mana yang paling tinggi?
2. Apakah lembur berkaitan dengan karyawan yang keluar?
3. Bagaimana hubungan pendapatan, masa kerja, work-life balance, kepuasan kerja, perjalanan dinas, dan jarak ke rumah dengan attrition?

## Dataset

- **Sumber:** [IBM HR Analytics Employee Attrition & Performance](https://www.kaggle.com/datasets/pavansubhasht/ibm-hr-analytics-attrition-dataset) di Kaggle
- **Ukuran:** 1.470 karyawan, 35 kolom (32 kolom setelah menghapus kolom konstan)
- **Catatan:** Dataset ini fiktif, dibuat oleh data scientist IBM. Hasil analisis di sini hanya menggambarkan pendekatan analisis dan tidak menggambarkan perusahaan nyata.
- File CSV mentah tidak disertakan di repositori ini. Silakan unduh langsung dari Kaggle.

## Alur Pengerjaan

1. **Pembersihan data (Power Query)**
   - Memeriksa tipe data, nilai kosong, dan duplikat ID karyawan (tidak ditemukan)
   - Menghapus kolom yang hanya berisi satu nilai (`EmployeeCount`, `Over18`, `StandardHours`)
2. **Rekayasa fitur**
   - Membuat `AttritionFlag` (1 = keluar, 0 = bertahan)
   - Membuat kelompok `AgeGroup`, `TenureGroup`, `IncomeBracket`, dan `DistanceGroup`
   - Menambahkan label yang mudah dibaca untuk kolom berkode angka (misalnya `JobSatisfaction` 1-4 menjadi Low sampai Very High)
3. **Pemodelan data**
   - Memakai *Sort by column* agar kelompok dan peringkat tampil berurutan secara logis, bukan alfabetis
   - Menyimpan semua measure di tabel khusus `Measures`
4. **Measure DAX** (lihat di bawah)
5. **Visualisasi**
   - Tiga halaman laporan dengan desain konsisten, slicer (Department, Gender, Age Group), dan navigasi antarhalaman
   - Palet warna yang konsisten dan judul visual yang berfokus pada insight

## Measure DAX Utama

```dax
Total Employees = COUNTROWS(HR_Employee)

Attrited Employees = SUM(HR_Employee[AttritionFlag])

Attrition Rate = DIVIDE([Attrited Employees], [Total Employees])

Attrition Rate (Overtime) =
CALCULATE([Attrition Rate], HR_Employee[OverTime] = "Yes")

Attrition Rate (No Overtime) =
CALCULATE([Attrition Rate], HR_Employee[OverTime] = "No")

Overtime % =
DIVIDE(
    CALCULATE([Total Employees], HR_Employee[OverTime] = "Yes"),
    [Total Employees]
)
```

## Temuan Utama

| Faktor | Kelompok dengan attrition lebih tinggi | Pembanding |
|---|---|---|
| Keseluruhan | 16,1% (237 dari 1.470 karyawan) | - |
| Lembur | 30,5% pada yang lembur | 10,4% pada yang tidak lembur |
| Masa kerja | 29,8% pada 0-2 tahun | 8,1% pada lebih dari 10 tahun |
| Pendapatan | 28,6% pada kelompok terendah | sekitar 10% pada dua kelompok tertinggi |
| Work-life balance | 31,3% saat dinilai "Bad" | 14,2% sampai 17,6% pada kelompok lain |
| Kepuasan kerja | 22,8% saat dinilai "Low" | 11,3% saat dinilai "Very High" |
| Perjalanan dinas | 24,9% pada yang sering bepergian | 8,0% pada yang tidak bepergian |
| Jarak ke rumah | 20,7% pada yang tinggal jauh | 13,6% pada yang tinggal dekat |
| Jabatan | 39,8% pada Sales Representative | 2,5% pada Research Director |

Pola lain yang menonjol: karyawan Sales yang lembur mencapai 37,5% attrition, dan karyawan di bawah 25 tahun mencapai 39,2%.

## Rekomendasi

1. **Tinjau beban lembur**, terutama di divisi Sales, dan pantau jam lembur sebagai indikator peringatan dini.
2. **Perkuat onboarding dan mentoring** pada dua tahun pertama masa kerja.
3. **Bandingkan gaji dengan standar pasar** untuk kelompok pendapatan terendah dan jabatan ber-attrition tinggi seperti Sales Representative.
4. **Tawarkan fleksibilitas**: opsi kerja hibrida, pembatasan perjalanan dinas, dan dukungan transportasi bagi karyawan yang tinggal jauh dari kantor.
5. **Lakukan survei kepuasan dan work-life balance secara berkala** dan tindak lanjuti skor yang rendah.

## Keterbatasan

- Dataset bersifat fiktif, dan temuan menunjukkan **keterkaitan, bukan sebab-akibat**.
- Beberapa kelompok berukuran kecil (misalnya jabatan tertentu atau kelompok usia termuda), sehingga persentasenya perlu dibaca bersama ukuran kelompoknya.
- Dataset tidak memiliki kolom tanggal, sehingga analisis tren waktu digantikan dengan analisis masa kerja.

## Cara Membuka Laporan

1. Unduh `IBM-HR-Attrition-Dashboard.pbix` dari repositori ini.
2. Buka dengan [Power BI Desktop](https://powerbi.microsoft.com/desktop/) (Windows).
3. Gunakan tombol halaman di kanan atas. Di Desktop, tahan **Ctrl** saat mengklik untuk berpindah halaman.
4. Ekspor PDF laporan juga tersedia: `IBM-HR-Attrition-Dashboard.pdf`.

## Tools

Power BI Desktop · Power Query · DAX

## Penulis

Alnanda Kusuma
Mahasiswa Informatika, Universitas Ahmad Dahlan
LinkedIn: https://www.linkedin.com/in/alnandakusuma/ · GitHub: https://github.com/alnandakusuma
