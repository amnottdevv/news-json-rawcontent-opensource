# filtered_content.json

**Versi:** 1.0.0
**Terakhir Diperbarui:** 2026-04-08

## 📖 Deskripsi

`filtered_content` adalah library tools berbasis JSON yang berfungsi sebagai database konten berbahaya (harmful content) untuk platform konten anime. Tools ini dirancang untuk diintegrasikan ke dalam backend atau frontend sistem rekomendasi guna mencegah konten dengan rating dewasa ekstrem, kekerasan grafis (gore), atau eksploitasi seksual tampil di area publik seperti **Homepage** atau **Halaman Rekomendasi Umum**.

Tools ini bersifat *powerful* dan *integrity-focused* karena menyediakan metadata spesifik (`reason`, `severity`, `category`) untuk pengambilan keputusan moderasi konten yang presisi.

## 🎯 Tujuan Penggunaan

- **Integritas Homepage:** Memastikan pengguna umum (termasuk pengunjung baru/anak di bawah umur) tidak terpapar konten traumatis.
- **Filtering Otomatis:** Digunakan oleh script untuk menyembunyikan konten berdasarkan `id` secara programatik.
- **Audit Konten:** Menyediakan alasan moderasi yang jelas untuk setiap entri yang diblokir.

## 📦 Struktur Data

File JSON terdiri dari tiga bagian utama: `metadata`, `filtered_content` (array), dan `stats`.

### 1. Metadata

| Field | Tipe Data | Deskripsi |
| :--- | :--- | :--- |
| `total_filtered` | `integer` | Jumlah total konten yang masuk daftar hitam saat ini. |
| `last_updated` | `string` | Tanggal sinkronisasi terakhir database (Format: YYYY-MM-DD). |
| `description` | `string` | Penjelasan singkat mengenai kriteria konten yang diblokir. |

### 2. Filtered Content Array (`filtered_content`)

Setiap objek dalam array mewakili satu entri konten yang diblokir.

| Field | Tipe Data | Deskripsi & Aturan Bisnis |
| :--- | :--- | :--- |
| `id` | `integer` | **Primary Key**. ID unik artikel/konten di database utama. |
| `title` | `string` | Judul asli konten. |
| `show_on_homepage` | `boolean` | Status flag. Nilai di tools ini **selalu `false`**. |
| `rating` | `string` | Klasifikasi usia resmi (umumnya "18+"). |
| `reason` | `string` | **Alasan moderasi**. Digunakan untuk log internal atau penjelasan jika diperlukan audit. |
| `block` | `boolean` | Flag aksi. **Selalu `true`**, menandakan blokir total dari halaman publik. |
| `category` | `string` | **Kategori Bahaya**. Sangat penting untuk analytics filtering. Lihat daftar kategori di bawah. |
| `severity` | `integer` | **Tingkat Keparahan (1-10)**. Semakin tinggi angka, semakin berat/berbahaya konten tersebut. |

#### 🏷️ Daftar Kategori (`category`)

| Nilai Kategori | Deskripsi | Contoh Judul |
| :--- | :--- | :--- |
| `gore` | Kekerasan fisik ekstrem, mutilasi, ledakan tubuh. | *Genocyber*, *Elfen Lied* |
| `sexual` | Konten dewasa/ecchi eksplisit, harem vulgar. | *High School DxD*, *Nukitashi* |
| `sexual_violence` | **Gabungan Berat**: Kekerasan Seksual (Pemerkosaan, Balas Dendam Seksual). | *Redo of Healer*, *Goblin Slayer* |
| `gore_sexual` | **Gabungan Berat**: Kekerasan grafis disertai pelecehan/nuansa seksual terhadap karakter (seringkali di bawah umur). | *Shoujo Tsubaki*, *Devilman Crybaby* |
| `sexual_gore` | Mirip dengan di atas, dengan penekanan eksploitasi seksual yang berujung pada kekerasan. | *Kite (1998)* |

### 3. Statistics (`stats`)

Bagian ini memberikan ringkasan cepat untuk dashboard moderasi atau pengecekan kesehatan data.

| Field | Deskripsi |
| :--- | :--- |
| `by_category` | Objek yang menghitung jumlah konten per kategori bahaya. |
| `by_severity` | Objek yang menghitung distribusi tingkat keparahan (misal: ada 8 konten dengan severity 10). |

## 🛠️ Cara Integrasi (Contoh Pseudocode)

Untuk menggunakan tools ini di aplikasi Anda, pastikan untuk memuat file JSON ini saat inisialisasi server atau sebagai fallback statis.

```javascript
// Contoh Logika Filter di Backend (Node.js / PHP / Python)
const filteredDB = require('./filtered_content.json');

function filterHomepageContent(allArticles) {
    // Ekstrak daftar ID yang diblokir untuk pencarian cepat
    const blockedIds = new Set(filteredDB.filtered_content.map(item => item.id));
    
    // Kembalikan artikel yang ID-nya TIDAK ADA di daftar blokir
    return allArticles.filter(article => !blockedIds.has(article.id));
}

// Contoh Penggunaan Lanjutan: Pengecekan Severity Tinggi
function isHighRiskContent(articleId) {
    const entry = filteredDB.filtered_content.find(item => item.id === articleId);
    if (entry && entry.severity >= 9) {
        console.warn(`PERINGATAN: Konten Severity ${entry.severity} terdeteksi - ${entry.reason}`);
        return true;
    }
    return false;
}
