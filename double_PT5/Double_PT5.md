# Double Posttest 5

> **Mata Kuliah:** Basis Data  
> **Topik:** AS (Alias) · IN / NOT IN · ORDER BY · DISTINCT · LIKE  
> **Database:** Sistem Supply Chain

---

## Skema Database 

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

Tampilkan data dari tabel `produk` dengan ketentuan alias berikut:

| Nama Kolom Asli | Alias yang Diinginkan |
|-----------------|-----------------------|
| `nama_produk`   | `Nama Barang`         |
| `kategori`      | `Jenis Produk`        |
| `harga_satuan`  | `Harga (Rp)`          |
| `stok`          | `Stok Tersedia`       |

**Contoh Output:**

| Nama Barang           | Jenis Produk | Harga (Rp)  | Stok Tersedia |
|-----------------------|-------------|------------:|:-------------:|
| Laptop Core i7        | Elektronik  | 12500000    | 50            |
| Mouse Wireless        | Aksesoris   |   150000    | 200           |
| Kabel HDMI 2m         | Aksesoris   |    75000    | 300           |
| SSD 1TB               | Elektronik  |  1200000    | 80            |
| Keyboard Mekanikal    | Aksesoris   |   850000    | 120           |
| Monitor 24 inch       | Elektronik  |  2800000    | 40            |
| Webcam HD             | Aksesoris   |   450000    | 90            |


---

### Soal 2 

Buat **dua query terpisah**:

- **Query A:** Tampilkan semua produk yang kategorinya `'Elektronik'` menggunakan `IN`
- **Query B:** Tampilkan semua produk yang kategorinya **bukan** `'Elektronik'` menggunakan `NOT IN`

**Contoh Output Query A** *(kategori IN 'Elektronik')*:

| id_produk | nama_produk     | kategori   | harga_satuan | stok |
|:---------:|-----------------|------------|-------------:|:----:|
| 1         | Laptop Core i7  | Elektronik |  12500000    |  50  |
| 4         | SSD 1TB         | Elektronik |   1200000    |  80  |
| 6         | Monitor 24 inch | Elektronik |   2800000    |  40  |

**Contoh Output Query B** *(kategori NOT IN 'Elektronik')*:

| id_produk | nama_produk        | kategori  | harga_satuan | stok |
|:---------:|--------------------|-----------|-------------:|:----:|
| 2         | Mouse Wireless     | Aksesoris |    150000    | 200  |
| 3         | Kabel HDMI 2m      | Aksesoris |     75000    | 300  |
| 5         | Keyboard Mekanikal | Aksesoris |    850000    | 120  |
| 7         | Webcam HD          | Aksesoris |    450000    |  90  |


---

### Soal 3 

Tampilkan **semua data pesanan** diurutkan berdasarkan:
1. `total_harga` dari yang **paling mahal ke paling murah** (DESC)
2. Jika `total_harga` sama, urutkan berdasarkan `tanggal_pesanan` dari yang **paling lama** (ASC)

**Contoh Output:**

| id_pesanan | id_pelanggan | tanggal_pesanan | status     | total_harga |
|:----------:|:------------:|-----------------|------------|------------:|
| 1          | 1            | 2024-01-10      | Selesai    | 12650000    |
| 6          | 4            | 2024-03-15      | Dikirim    |  3250000    |
| 2          | 2            | 2024-01-15      | Dikirim    |  1275000    |
| 5          | 2            | 2024-03-05      | Dibatalkan |  1200000    |
| 3          | 3            | 2024-02-01      | Diproses   |   925000    |
| 4          | 1            | 2024-02-10      | Selesai    |   150000    |

---

### Soal 4

Buat **dua query terpisah** menggunakan `DISTINCT`:

- **Query A:** Tampilkan daftar **kota unik** dari tabel `pelanggan`
- **Query B:** Tampilkan daftar **kota unik** dari tabel `supplier`

**Contoh Output Query A** *(kota pelanggan)*:

| kota     |
|----------|
| Bandung  |
| Surabaya |
| Jakarta  |
| Medan    |

**Contoh Output Query B** *(kota supplier)*:

| kota      |
|-----------|
| Surabaya  |
| Jakarta   |
| Singapore |
| Bandung   |

---

### Soal 5

Buat **dua query terpisah** untuk mencari data di tabel `supplier`:

- **Query A:** Cari supplier yang nama nya **diawali** kata `'PT'`
- **Query B:** Cari supplier yang nama nya **mengandung** kata `'Logistik'` di bagian manapun

**Contoh Output Query A** *(nama diawali 'PT')*:

| id_supplier | nama_supplier    | kota     | negara    |
|:-----------:|------------------|----------|-----------|
| 1           | PT Maju Jaya     | Surabaya | Indonesia |
| 4           | PT Karya Mandiri | Bandung  | Indonesia |

**Contoh Output Query B** *(nama mengandung 'Logistik')*:

| id_supplier | nama_supplier      | kota    | negara    |
|:-----------:|--------------------|---------|-----------|
| 2           | CV Sinar Logistik  | Jakarta | Indonesia |
---
