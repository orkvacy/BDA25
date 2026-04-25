# Double Posttest 6

> **Mata Kuliah:** Basis Data  
> **Topik:** COUNT · SUM · AVG · MIN · MAX · GROUP BY · HAVING · CASE  
> **Database:** Sistem Supply Chain

---

## Skema Database (Ringkas)

```
supplier    : id_supplier, nama_supplier, kota, negara, no_telepon
produk      : id_produk, nama_produk, kategori, harga_satuan, stok, id_supplier (FK)
pelanggan   : id_pelanggan, nama_pelanggan, kota, negara, email
pesanan     : id_pesanan, id_pelanggan (FK), tanggal_pesanan, status, total_harga
```

---

## Data Awal

```sql
INSERT INTO supplier (nama_supplier, kota, negara, no_telepon) VALUES
('PT Maju Jaya',       'Surabaya',  'Indonesia', '031-5551234'),
('CV Sinar Logistik',  'Jakarta',   'Indonesia', '021-7778899'),
('Global Parts Ltd',   'Singapore', 'Singapore', '+65-62345678'),
('PT Karya Mandiri',   'Bandung',   'Indonesia', '022-4443322');

INSERT INTO produk (nama_produk, kategori, harga_satuan, stok, id_supplier) VALUES
('Laptop Core i7',     'Elektronik', 12500000,  50, 1),
('Mouse Wireless',     'Aksesoris',    150000, 200, 1),
('Kabel HDMI 2m',      'Aksesoris',     75000, 300, 2),
('SSD 1TB',            'Elektronik',  1200000,  80, 3),
('Keyboard Mekanikal', 'Aksesoris',    850000, 120, 2),
('Monitor 24 inch',    'Elektronik',  2800000,  40, 1),
('Webcam HD',          'Aksesoris',    450000,  90, NULL);

INSERT INTO pelanggan (nama_pelanggan, kota, negara, email) VALUES
('Budi Santoso', 'Bandung',  'Indonesia', 'budi@email.com'),
('Siti Rahayu',  'Surabaya', 'Indonesia', 'siti@email.com'),
('Ahmad Fauzi',  'Jakarta',  'Indonesia', 'ahmad@email.com'),
('Dewi Lestari', 'Medan',    'Indonesia', 'dewi@email.com');

INSERT INTO pesanan (id_pelanggan, tanggal_pesanan, status, total_harga) VALUES
(1, '2024-01-10', 'Selesai',    12650000),
(2, '2024-01-15', 'Dikirim',     1275000),
(3, '2024-02-01', 'Diproses',     925000),
(1, '2024-02-10', 'Selesai',      150000),
(2, '2024-03-05', 'Dibatalkan',  1200000),
(4, '2024-03-15', 'Dikirim',     3250000);
```

---

## Soal

---

### Soal 1

Buat **dua query terpisah**:

- **Query A:** Hitung **total seluruh pesanan** yang ada di tabel `pesanan`
- **Query B:** Hitung **jumlah pesanan per status** menggunakan `GROUP BY`

**Contoh Output Query A:**

| total_pesanan |
|:-------------:|
| 6             |

**Contoh Output Query B:**

| status     | jumlah_pesanan |
|------------|:--------------:|
| Dibatalkan | 1              |
| Dikirim    | 2              |
| Diproses   | 1              |
| Selesai    | 2              |

---

### Soal 2 

Hitung **total nilai pesanan** dan **rata-rata nilai pesanan** untuk setiap pelanggan dari tabel `pesanan`. Kelompokkan berdasarkan `id_pelanggan` dan urutkan berdasarkan `total_belanja` dari **terbesar ke terkecil**.

**Contoh Output:**

| id_pelanggan | total_belanja | rata_rata_belanja |
|:------------:|-------------:|------------------:|
| 1            | 12800000      | 6400000.000000    |
| 4            |  3250000      | 3250000.000000    |
| 2            |  2475000      | 1237500.000000    |
| 3            |   925000      |  925000.000000    |

---

### Soal 3

Hitung **rata-rata harga produk** berdasarkan `kategori` dari tabel `produk`.  
Tampilkan nama kategori dan rata-rata harganya, diurutkan dari harga tertinggi.

**Contoh Output:**

| kategori   | rata_harga        |
|------------|------------------:|
| Elektronik | 5500000.000000    |
| Aksesoris  |  381250.000000    |


---

### Soal 4 

Tampilkan **harga produk tertinggi** dan **harga produk terendah** berdasarkan `kategori`.

**Contoh Output:**

| kategori   | harga_tertinggi | harga_terendah |
|------------|----------------:|---------------:|
| Aksesoris  |         850000  |         75000  |
| Elektronik |       12500000  |       1200000  |

---

### Soal 5 

Buat **dua query terpisah**:

**Query A:** Tampilkan kategori produk yang memiliki **total stok lebih dari 200 unit**, diurutkan dari total stok terbesar.

**Contoh Output Query A:**

| kategori  | total_stok |
|-----------|:----------:|
| Aksesoris | 710        |


**Query B:** Dari tabel `produk`, buat kolom baru `Segmen Harga` menggunakan `CASE`:
- Harga < 200.000          → `'Murah'`
- Harga 200.000 – 999.999  → `'Menengah'`
- Harga ≥ 1.000.000        → `'Mahal'`

**Contoh Output Query B:**

| nama_produk        | harga_satuan | Segmen Harga |
|--------------------|-------------:|:------------:|
| Laptop Core i7     |  12500000    | Mahal        |
| Mouse Wireless     |    150000    | Murah        |
| Kabel HDMI 2m      |     75000    | Murah        |
| SSD 1TB            |   1200000    | Mahal        |
| Keyboard Mekanikal |    850000    | Menengah     |
| Monitor 24 inch    |   2800000    | Mahal        |
| Webcam HD          |    450000    | Menengah     |

---
