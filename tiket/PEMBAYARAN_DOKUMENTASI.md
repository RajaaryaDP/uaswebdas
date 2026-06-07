# Dokumentasi Sistem Pembayaran

## Cara Kerja

### 1. **Alur Pembayaran**
- **Step 1 (Pilihan Anda)**: User memilih paket wisata di halaman tiket
- **Step 2 (Masukan Rincian)**: User mengisi form data diri dan jumlah peserta
- **Step 3 (Pembayaran)**: User melakukan pembayaran melalui halaman pembayaran

### 2. **Logika Perhitungan Harga**

#### Harga Dasar Per Peserta
- **$480** per peserta

#### Penghitungan Total
```
Subtotal = Jumlah Peserta × $480

Jika Jumlah Peserta >= 3:
  Diskon = Subtotal × 15%
  Total = Subtotal - Diskon
Else:
  Total = Subtotal
```

#### Contoh:
- **1 Peserta**: 1 × $480 = $480 (tanpa diskon)
- **2 Peserta**: 2 × $480 = $960 (tanpa diskon)  
- **3 Peserta**: 3 × $480 = $1,440 - ($1,440 × 15%) = **$1,224** ✅ (diskon otomatis)
- **4 Peserta**: 4 × $480 = $1,920 - ($1,920 × 15%) = **$1,632** ✅ (diskon otomatis)

### 3. **Flow Data**

#### Dari Form Data → Pembayaran
```
Form Data (tiket1.html, tiket2.html, tiket3.html)
    ↓
    (Validasi Form)
    ↓
    (Simpan ke localStorage)
    ↓
    Redirect ke pembayaran.html
    ↓
Pembayaran Page
    (Baca data dari localStorage)
    ↓
    (Hitung Total Pembayaran)
    ↓
    (Tampilkan Summary & Form Pembayaran)
```

### 4. **Data yang Disimpan**

#### localStorage Key: `formData`
```json
{
  "namaLengkap": "John Doe",
  "alamatEmail": "john@example.com",
  "negaraWilayah": "ID",
  "nomorTelepon": "+62 812345678",
  "jumlahPeserta": 3,
  "nomorTeleponPeserta": "+62 987654321"
}
```

#### localStorage Key: `paymentData`
```json
{
  "jumlahPeserta": 3,
  "subtotal": 1440,
  "diskon": 216,
  "total": 1224,
  "isDiskonEligible": true
}
```

#### localStorage Key: `paymentSubmitted` (setelah pembayaran)
```json
{
  "kartunama": "JOHN DOE",
  "kartuNomor": "1234567890123456",
  "kartuBulan": "12",
  "kartuTahun": "25",
  "kartuCvv": "123",
  "email": "john@example.com",
  "agreeTerms": true,
  "timestamp": "2024-06-07T10:30:00Z",
  "paymentData": { ... }
}
```

## File-File yang Dibuat/Dimodifikasi

### File Baru
- `tiket/pembayaran.html` - Halaman pembayaran
- `tiket/pembayaran.css` - CSS untuk halaman pembayaran

### File yang Dimodifikasi
- `tiket/tiket1.html` - Menambahkan form submit handler & localStorage
- `tiket/tiket2.html` - Menambahkan form submit handler & localStorage
- `tiket/tiket3.html` - Menambahkan form submit handler & localStorage

## Fitur-Fitur

### ✅ Summary Card
- Menampilkan nama paket wisata
- Menampilkan durasi paket
- Menampilkan harga per peserta
- Menampilkan jumlah peserta (real-time update)
- Menampilkan subtotal
- Menampilkan diskon (jika eligible)
- Menampilkan total pembayaran (dengan diskon badge)

### ✅ Payment Form
- Input nama pemilik kartu
- Input nomor kartu kredit (auto-format: XXXX XXXX XXXX XXXX)
- Input tanggal kadaluarsa (bulan/tahun)
- Input CVV (3 digit)
- Input email untuk konfirmasi
- Checkbox persetujuan terms & conditions
- Tombol "Kembali" untuk back to form
- Tombol "Bayar Sekarang" untuk submit pembayaran

### ✅ Validasi
- Semua field harus terisi
- Format email harus valid
- Nomor kartu harus 16 digit
- CVV harus 3 digit
- Checkbox terms harus dicentang

### ✅ Step Indicator
- Menunjukkan progress: Step 1 ✓ → Step 2 ✓ → Step 3 (active)

## Cara Testing

1. Buka halaman tiket (tiket1.html, tiket2.html, atau tiket3.html)
2. Isi form dengan data:
   - Nama: Isi dengan nama
   - Email: Isi dengan email valid
   - Negara: Pilih negara
   - Nomor Telepon: Isi dengan nomor
   - **Jumlah Peserta: Coba dengan 1, 2, 3, 4 untuk melihat diskon bekerja**
   - Nomor Telepon Peserta: Isi dengan nomor
3. Klik "Simpan"
4. Lihat halaman pembayaran:
   - Summary akan update berdasarkan jumlah peserta
   - Jika peserta >= 3, diskon 15% akan muncul otomatis
5. Isi form pembayaran dan klik "Bayar Sekarang"
6. Lihat pesan sukses dan redirect ke home

## Catatan Penting

⚠️ **Demo Mode**: Sistem pembayaran ini adalah demo/prototype. Data pembayaran disimpan di localStorage saja (tidak ke server). Untuk production, integrasikan dengan payment gateway seperti:
- Stripe
- PayPal
- Midtrans
- DOKU
- dll

---

**Dibuat**: 7 Juni 2026
**Status**: ✅ Selesai dan Siap Digunakan
