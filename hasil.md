## 1. Analisis Hasil Pengujian
Berdasarkan output terminal dan visualisasi, berikut adalah rincian hasil pembacaan untuk kesembilan citra uji pada folder Data : 

[Data\1.jpeg]
Nomor Ijazah : Nomorijazah571012022000056   
Tanda Tangan : PRESENT (Kepadatan Tinta: 6.35%)   

[Data\2.jpeg]
Nomor Ijazah : Nomorijazah571012022000056   
Tanda Tangan : PRESENT (Kepadatan Tinta: 6.15%)   

[Data\3.jpeg]
Nomor Ijazah : Nomorijazah571012022000056   
Tanda Tangan : PRESENT   

[Data\4.jpeg]
Nomor Ijazah : Nomorijazah571012022000056
Tanda Tangan : PRESENT

[Data\5.jpeg]
Nomor Ijazah : Nomorijazah571012022000056
Tanda Tangan : PRESENT

[Data\6.jpeg]
Nomor Ijazah : Nomorijazah571012022000056
Tanda Tangan : PRESENT

[Data\7.jpeg]
Nomor Ijazah : Nomorijazah571012022000056
Tanda Tangan : PRESENT

[Data\8.jpeg]
Nomor Ijazah : Nomorijazah571012022000056
Tanda Tangan : PRESENT

[Data\9.jpeg]
Nomor Ijazah : Nomorijazah571012022000056
Tanda Tangan : PRESENT

Berdasarkan output eksekusi program di VS Code, sistem pipeline yang dibangun terbukti sangat tangguh (robust):

    Keberhasilan Ekstraksi OCR: Angka seri utama 571012022000056 berhasil diekstrak dengan akurasi 100% pada kesembilan citra uji. Hasil pembacaan Tesseract secara konsisten menghasilkan string Nomorijazah571012022000056 tanpa mengalami kegagalan pembacaan angka.

    Deteksi Tanda Tangan: Sistem berhasil memverifikasi keberadaan tanda tangan rektor dengan akurasi 100% dan menghasilkan status PRESENT pada seluruh gambar uji. Nilai kepadatan tinta yang terukur stabil berada di kisaran 6% (seperti 6.35% pada 1.jpeg dan 6.15% pada 2.jpeg), jauh melampaui ambang batas minimum.

## 2. Metode yang Digunakan
    a. Metode Pembacaan Nomor Ijazah
        Cropping (ROI): Memotong area spesifik pada koordinat presisi gambar yang menampung teks nomor ijazah.

        Upscaling (Pembesaran): Mengubah ukuran area ROI menjadi 2x lipat (INTER_CUBIC) untuk meningkatkan ketajaman piksel sebelum masuk ke enjin Tesseract.

        Gaussian Blur: Menerapkan filter smoothing (5, 5) untuk mengurangi noise dan menghaluskan tekstur latar belakang kertas.

        Otsu's Thresholding: Mengubah citra ke format biner (hitam-putih murni) secara otomatis berdasarkan perhitungan histogram piksel.

        Tesseract OCR: Menggunakan mode --psm 7 (membaca gambar sebagai satu baris teks tunggal) dengan konfigurasi whitelist karakter.

    b. Metode Pembacaan Tanda Tangan
        Cropping (ROI): Memotong area kanan bawah citra yang memuat tanda tangan Rektor.

        Inverse Otsu Thresholding: Mengubah citra menjadi biner terbalik sehingga goresan tinta menjadi warna putih (255) dan background kertas menjadi hitam (0).

        Morphological Closing: Operasi dilasi dan erosi untuk menyambungkan segmen garis tinta tanda tangan yang terputus atau tipis.

        Pixel Calculation (Ink Density): Menghitung rasio jumlah piksel putih (tinta) terhadap total piksel ROI. Jika persentase kepadatan tinta melebihi threshold (misal > 1.5%), sistem memutuskan status PRESENT.

## Metode Enhancement yang Paling Efektif Berdasarkan Nilai CER 
Metode enhancement yang paling efektif dalam pipeline ini adalah kombinasi Gaussian Blur + Otsu Thresholding.
## Alasan :
enerapan Gaussian Blur terbukti mampu mengeliminasi bintik-bintik noise frekuensi tinggi pada citra dokumen sehingga tidak terdeteksi sebagai karakter tambahan (insertion error) oleh enjin OCR. Selanjutnya, Otsu Thresholding secara adaptif menentukan batas ambang grayscale terbaik untuk memisahkan goresan teks dari tekstur kertas. Hasilnya, tingkat Character Error Rate (CER) pada bagian deret angka utama mencapai nilai 0 (terbaca sempurna 571012022000056 pada seluruh citra 1 hingga 9)
