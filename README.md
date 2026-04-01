# 🇮🇩 Indonesia Tech News JSON (Open Source)

Dataset berita teknologi Indonesia dalam format JSON yang bisa digunakan secara bebas untuk project web, API, atau aplikasi lainnya.

---

## 📌 About

Repository ini berisi kumpulan berita teknologi Indonesia yang disimpan dalam format JSON.

JSON adalah format data terbuka yang ringan dan mudah dibaca oleh manusia maupun mesin, sering digunakan untuk pertukaran data di web :contentReference[oaicite:0]{index=0}.

Project ini dibuat untuk:
- Developer web / frontend
- Project portfolio
- Web berita sederhana (tanpa backend)
- Testing API / UI

---

## 🚀 Features

- 📰 Berita teknologi Indonesia
- 📅 Data terstruktur (judul, tanggal, isi, dll)
- 🏷️ Support kategori / topic
- 🖼️ Support gambar (image URL)
- 🌐 Bisa diakses via GitHub Raw (tanpa backend)

---

## 📂 Struktur Data

Contoh JSON:

```json
{
  "news": [
    {
      "id": 1,
      "name": "Judul Berita",
      "artikel" : "bla bla",
      "posted": "2026-04-01",
      "descriptions": "Deskripsi berita...",
      "image": "https://example.com/image.jpg",
      "topics": ["tech", "cyber"]
    }
  ]
}
