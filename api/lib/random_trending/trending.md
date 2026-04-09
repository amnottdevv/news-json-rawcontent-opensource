# trending.json

**Versi:** 1.0.0
**Format:** JSON Array
**Sumber:** `https://raw.githubusercontent.com/amnottdevv/news-json-rawcontent-opensource/refs/heads/main/api/lib/random_trending/trending.json`

---

## 📖 Deskripsi

`trending.json` adalah dataset skor popularitas konten yang digunakan untuk sistem rekomendasi dan algoritma *trending*. File ini menyimpan nilai `score` untuk setiap konten berdasarkan metrik engagement (views, likes, shares) yang dihitung secara periodik.

Dataset ini dirancang sebagai *single source of truth* untuk menentukan urutan konten pada halaman *Trending*, *Popular Now*, atau widget rekomendasi berbasis popularitas.

---

## 📦 Struktur Data

File berbentuk **array of objects** tanpa wrapper tambahan.

```json
[
  {
    "id": 1,
    "score": 655
  },
  {
    "id": 2,
    "score": 115
  }
]
```
Field Specification
Field	Tipe	Deskripsi
id	integer	Unique identifier konten (primary key).
score	integer	Skor popularitas kumulatif. Semakin tinggi, semakin populer.
fetch data
```javascript
const TRENDING_URL = 'https://raw.githubusercontent.com/amnottdevv/news-json-rawcontent-opensource/refs/heads/main/api/lib/random_trending/trending.json';

async function fetchTrendingScores() {
    const response = await fetch(TRENDING_URL);
    const data = await response.json();
    return data;
}
```
use : ```https://raw.githubusercontent.com/amnottdevv/news-json-rawcontent-opensource/refs/heads/main/api/lib/random_trending/trending.json```
