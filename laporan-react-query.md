# Optimasi Performa React dengan Caching

**Nama:** Ahmad Alaudin Falah  
**NIM:** V3424082  
**Kelas:** TIC 24  
**Mata Kuliah:** Pemrograman Front-End  

---

## Tujuan

Proyek ini bertujuan untuk menganalisis perbedaan performa aplikasi **Point of Sales (POS)** antara implementasi **tanpa cache** dan **dengan cache**, serta menilai dampaknya terhadap efisiensi dan pengalaman pengguna.

---

## Latar Belakang

Caching merupakan teknik penyimpanan data sementara untuk mempercepat proses akses data yang sering digunakan.  
Dalam aplikasi React, caching dapat membantu mengurangi waktu render dan beban komputasi ketika melakukan operasi berulang, seperti pencarian produk dalam sistem Point of Sales.

---

## Metodologi

- **Aplikasi yang diuji:** Point of Sales dengan **10.000 data produk**  
- **Skenario pengujian:** Melakukan pencarian berulang untuk `"produk 5000"`  
- **Tools yang digunakan:**
  - React DevTools Profiler  
  - Browser Console  
- **Metrik yang diukur:**
  - Waktu render  
  - Konsistensi hasil  
  - Beban komputasi CPU  

---

## Versi Tanpa Cache

### Karakteristik
- Setiap pencarian dilakukan dari awal tanpa penyimpanan hasil sebelumnya  
- Beban CPU dan waktu render tetap konstan  
- Tidak ada optimasi memori  

### Hasil Profiling
**Console Log:**  
![No Cache Console](./images/NoCacheConsole.png)  

**React Profiler:**  
![No Cache Profiler](./images/NoCacheProfiler.png)  

| Pengujian | Waktu Render |
|------------|--------------|
| Pencarian Pertama | 1.0 ms |
| Pencarian Berulang | 1.0 ms |

---

## Versi Dengan Cache

### Karakteristik
- Hasil pencarian disimpan dalam **cache memory (Map)**  
- Pencarian berulang lebih cepat (cache hit)  
- Beban CPU menurun drastis  

### Hasil Profiling
**Console Log:**  
![Cache Console](./images/CacheConsole.png)  

**React Profiler:**  
![Cache Profiler](./images/CacheProfiler.png)  

| Pengujian | Waktu Render |
|------------|--------------|
| Pencarian Pertama (Cache MISS) | 1.0 ms |
| Pencarian Ulang (Cache HIT) | 0.4 ms |

 **Peningkatan performa:** sekitar **60% lebih cepat**

---

## Implementasi Caching

Berikut contoh implementasi sederhana caching menggunakan **Map** di React:

```javascript
// cache.js
const searchCache = new Map();
const MAX_CACHE_SIZE = 50;

export const getFromCache = (key) => {
  return searchCache.get(key);
};

export const setToCache = (key, data) => {
  if (searchCache.size >= MAX_CACHE_SIZE) {
    const firstKey = searchCache.keys().next().value;
    searchCache.delete(firstKey);
  }
  searchCache.set(key, data);
};
