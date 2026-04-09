# 📚 RINA (Rating ID News API)

**RINA** adalah API ringan berbasis JSON untuk mengelola **rating usia** pada setiap berita. API ini dirancang untuk membantu developer menerapkan **filter konten berbasis umur** secara sederhana tanpa backend.

---

## 🎯 Tujuan Utama

- Menyaring konten **dewasa (18+)** dari halaman publik
- Melindungi pengguna di bawah umur
- Meningkatkan kredibilitas & kepatuhan platform
- Memberikan kontrol penuh terhadap distribusi konten

---

## ⚙️ Cara Kerja Singkat

Setiap berita memiliki:
- `id`
- `min_age`
- `rating`

Frontend cukup:
1. Fetch data RINA
2. Cocokkan dengan ID berita
3. Filter sesuai kebutuhan (misalnya hanya `SU`)

---

## 📊 Struktur Data

```json
{
  "metadata": {
    "total_rated": 315,
    "last_updated": "2026-04-09",
    "description": "Rating usia untuk setiap ID berita"
  },
  "ratings": [
    { "id": 1, "min_age": 13, "rating": "SU" },
    { "id": 176, "min_age": 18, "rating": "D" }
  ]
}
```
## endpoint
| Field     | Tipe    | Deskripsi              |
| --------- | ------- | ---------------------- |
| `id`      | Integer | ID unik berita         |
| `min_age` | Integer | Usia minimum akses     |
| `rating`  | String  | `SU` (13+) / `D` (18+) |

## use api 
***api*** : 
```bash
https://raw.githubusercontent.com/amnottdevv/news-json-rawcontent-opensource/refs/heads/main/api/lib/RINA/rating_ID_news_api.json
```
## contoh implentasi
***contoh implentasi javascript***
```javascript
const RINA_URL = 'https://raw.githubusercontent.com/amnottdevv/news-json-rawcontent-opensource/refs/heads/main/api/lib/RINA/rating_ID_news_api.json';

async function getRatings() {
  const res = await fetch(RINA_URL);
  const data = await res.json();
  return data.ratings;
}

function getRating(ratings, id) {
  return ratings.find(r => r.id === id) || { rating: 'SU', min_age: 13 };
}

async function filterSafeNews(newsList) {
  const ratings = await getRatings();

  return newsList.filter(news => {
    const r = getRating(ratings, news.id);
    return r.rating === 'SU';
  });
}
```
🔒 Contoh Use Case
- 1. Homepage Aman

Hanya tampilkan berita dengan rating SU

- 2. Gate Konten Dewasa

Jika rating === "D":

tampilkan modal verifikasi umur
redirect jika lolos
- 3. Parental Mode

Blok semua konten D secara permanen

🚀 Kelebihan
Super ringan & cepat
Plug & play
Cocok untuk static website
Open source & bebas pakai
let's code! 
