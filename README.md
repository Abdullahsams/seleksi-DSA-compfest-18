# Seleksi Data Science Academy — COMPFEST 18

Submission untuk tahap seleksi peserta Data Science Academy COMPFEST 18: memprediksi kandungan bahan organik tanah dari data geografis dan spektral tanah (reduksi dimensi PCA), lalu menjawab serangkaian research question berdasarkan dataset tersebut.

Repo ini isinya jawaban tertulis saja. Dataset dan notebook (training + inference) ada di Kaggle, bukan di-mirror ke sini:

- **Kompetisi & dataset**: https://www.kaggle.com/t/ec937e759606422a8fd7f5b9e6794680
- **Notebook**: https://www.kaggle.com/code/abdullahsamsudi/seleksi-dsa-compfest-18
- **Soal seleksi asli**: [`Tugas Data Science Academy COMPFEST 18.pdf`](<./Tugas Data Science Academy COMPFEST 18.pdf>)
- **Jawaban research questions**: [`Seleksi - Raja Jawa.pdf`](<./Seleksi - Raja Jawa.pdf>)

## Konteks

Tanah adalah aset vital pertanian dan ketahanan pangan, dan kandungan bahan organik adalah salah satu indikator kesuburan tanah paling krusial. Uji lab untuk mengukurnya mahal dan lambat, sehingga kompetisi ini menantang peserta membangun model ML yang memprediksi kandungan organik tanah dari data koordinat, zona geografis, dan dua blok fitur spektral (optik dan MIR) tanpa perlu uji lab langsung.

Target: `property_organic_content`, metrik penilaian: RMSE. Tantangan utama datasetnya adalah blok spektral `spectral_band_B` (MIR — paling sensitif terhadap bahan organik) yang missing 85–89% karena perbedaan alat lab antar sumber data.

## Ringkasan pendekatan

- **Feature engineering**: rasio kimiawi (Ca/Mg, Na/CEC, base saturation, particle coarse/fine), group-stats per biome & zona geografis, target encoding spasial (macro/meso/micro/biome) dengan skema out-of-fold.
- **Model**: ensemble XGBoost + LightGBM (Huber loss) + CatBoost (Huber loss), diblend via RidgeCV. Tree-based dipilih karena native NaN handling — cocok untuk `spectral_band_B` yang sparse, tanpa perlu imputasi eksplisit.
- **Target**: winsorized di persentil 99 sebelum training (distribusi target skewed berat, skewness 2.03).
- **Hasil**: OOF RMSE 11.18 (blend); target `property_organic_content` sendiri berkisar ~2–195.

Detail lengkap (feature engineering, model selection, evaluasi metrik, dll) ada di [jawaban research questions](<./Seleksi - Raja Jawa.pdf>).

## Isi repo

- `README.md` — halaman ini.
- `Tugas Data Science Academy COMPFEST 18.pdf` — soal seleksi asli dari panitia (bobot penilaian, deskripsi kompetisi, daftar 10 research question).
- `Seleksi - Raja Jawa.pdf` — jawaban 10 research question, dinilai 90% dari total seleksi (10% sisanya dari skor Kaggle).

