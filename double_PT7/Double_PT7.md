# Double Posttest 7

> **Mata Kuliah:** Basis Data  
> **Topik:** INNER JOIN · LEFT JOIN · RIGHT JOIN · FULL OUTER JOIN  
> **Database:** Sistem Supply Chain

---

## Skema Database

### Tabel `supplier`
```sql
CREATE TABLE supplier (
    id_supplier   INT AUTO_INCREMENT PRIMARY KEY,
    nama_supplier VARCHAR(100) NOT NULL,
    kota          VARCHAR(50),
    negara        VARCHAR(50),
    no_telepon    VARCHAR(20)
);
```

### Tabel `produk`
```sql
CREATE TABLE produk (
    id_produk    INT AUTO_INCREMENT PRIMARY KEY,
    nama_produk  VARCHAR(100) NOT NULL,
    kategori     VARCHAR(50),
    harga_satuan DECIMAL(12,2) NOT NULL,
    stok         INT DEFAULT 0,
    id_supplier  INT,
    FOREIGN KEY (id_supplier) REFERENCES supplier(id_supplier)
);
```

### Tabel `pelanggan`
```sql
CREATE TABLE pelanggan (
    id_pelanggan   INT AUTO_INCREMENT PRIMARY KEY,
    nama_pelanggan VARCHAR(100) NOT NULL,
    kota           VARCHAR(50),
    negara         VARCHAR(50),
    email          VARCHAR(100)
);
```

### Tabel `pesanan`
```sql
CREATE TABLE pesanan (
    id_pesanan      INT AUTO_INCREMENT PRIMARY KEY,
    id_pelanggan    INT,
    tanggal_pesanan DATE,
    status          VARCHAR(30),
    total_harga     DECIMAL(14,2),
    FOREIGN KEY (id_pelanggan) REFERENCES pelanggan(id_pelanggan)
);
```

### Tabel `pengiriman`
```sql
CREATE TABLE pengiriman (
    id_pengiriman INT AUTO_INCREMENT PRIMARY KEY,
    id_pesanan    INT,
    tanggal_kirim DATE,
    tanggal_tiba  DATE,
    status_kirim  VARCHAR(30),
    FOREIGN KEY (id_pesanan) REFERENCES pesanan(id_pesanan)
);
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

INSERT INTO pengiriman (id_pesanan, tanggal_kirim, tanggal_tiba, status_kirim) VALUES
(1, '2024-01-12', '2024-01-14', 'Tiba'),
(2, '2024-01-17', NULL,         'Dalam Perjalanan'),
(6, '2024-03-17', '2024-03-20', 'Tiba');
```

---

## Soal

---

### Soal 1

Tampilkan detail pesanan yang berstatus **`'Selesai'`** dengan menggabungkan tabel `pesanan` dan `pelanggan`.

Kolom yang ditampilkan: `id_pesanan`, `tanggal_pesanan`, `total_harga` *(dari pesanan)*, `nama_pelanggan`, `kota` *(dari pelanggan)*

**Contoh Output:**

| id_pesanan | tanggal_pesanan | total_harga | nama_pelanggan | kota    |
|:----------:|-----------------|------------:|----------------|---------|
| 1          | 2024-01-10      | 12650000    | Budi Santoso   | Bandung |
| 4          | 2024-02-10      |   150000    | Budi Santoso   | Bandung |

---

### Soal 2 

Tampilkan **semua produk** beserta informasi supplier yang menyediakannya.  
Produk yang **tidak memiliki supplier** tetap harus muncul dengan nilai `NULL` pada kolom supplier.

Kolom yang ditampilkan: `nama_produk`, `kategori`, `harga_satuan`, `nama_supplier`, `kota` *(dari supplier)*

**Contoh Output:**

| nama_produk        | kategori   | harga_satuan | nama_supplier      | kota      |
|--------------------|------------|-------------:|--------------------|-----------|
| Laptop Core i7     | Elektronik |  12500000    | PT Maju Jaya       | Surabaya  |
| Mouse Wireless     | Aksesoris  |    150000    | PT Maju Jaya       | Surabaya  |
| Kabel HDMI 2m      | Aksesoris  |     75000    | CV Sinar Logistik  | Jakarta   |
| SSD 1TB            | Elektronik |   1200000    | Global Parts Ltd   | Singapore |
| Keyboard Mekanikal | Aksesoris  |    850000    | CV Sinar Logistik  | Jakarta   |
| Monitor 24 inch    | Elektronik |   2800000    | PT Maju Jaya       | Surabaya  |
| Webcam HD          | Aksesoris  |    450000    | NULL               | NULL      |

---

### Soal 3

Tampilkan **semua supplier** beserta produk yang mereka sediakan.  
Supplier yang **belum memiliki produk** tetap harus muncul dengan nilai `NULL` pada kolom produk.

Kolom yang ditampilkan: `nama_supplier`, `kota` *(dari supplier)*, `nama_produk`, `stok`

**Contoh Output:**

| nama_supplier      | kota      | nama_produk        | stok |
|--------------------|-----------|--------------------|:----:|
| PT Maju Jaya       | Surabaya  | Laptop Core i7     |  50  |
| PT Maju Jaya       | Surabaya  | Mouse Wireless     | 200  |
| PT Maju Jaya       | Surabaya  | Monitor 24 inch    |  40  |
| CV Sinar Logistik  | Jakarta   | Kabel HDMI 2m      | 300  |
| CV Sinar Logistik  | Jakarta   | Keyboard Mekanikal | 120  |
| Global Parts Ltd   | Singapore | SSD 1TB            |  80  |
| PT Karya Mandiri   | Bandung   | NULL               | NULL |

---

### Soal 4 

Buat laporan pengiriman lengkap dengan menggabungkan **3 tabel** sekaligus: `pesanan`, `pelanggan`, dan `pengiriman`.

Kolom yang ditampilkan:
- `id_pesanan`, `status` *(dari pesanan)*
- `nama_pelanggan` *(dari pelanggan)*
- `tanggal_kirim`, `tanggal_tiba`, `status_kirim` *(dari pengiriman)*

Urutkan berdasarkan `id_pesanan` secara **ascending**.

**Contoh Output:**

| id_pesanan | status   | nama_pelanggan | tanggal_kirim | tanggal_tiba | status_kirim      |
|:----------:|----------|----------------|---------------|--------------|-------------------|
| 1          | Selesai  | Budi Santoso   | 2024-01-12    | 2024-01-14   | Tiba              |
| 2          | Dikirim  | Siti Rahayu    | 2024-01-17    | NULL         | Dalam Perjalanan  |
| 6          | Dikirim  | Dewi Lestari   | 2024-03-17    | 2024-03-20   | Tiba              |

---

### Soal 5 

MySQL tidak mendukung `FULL OUTER JOIN` secara langsung.  
Simulasikan **FULL OUTER JOIN** antara tabel `produk` dan `supplier` menggunakan kombinasi `LEFT JOIN` dan `RIGHT JOIN` dengan `UNION`.

Tampilkan semua produk (termasuk yang tidak punya supplier) **DAN** semua supplier (termasuk yang tidak punya produk).

Kolom yang ditampilkan: `nama_produk`, `kategori`, `nama_supplier`, `kota` *(dari supplier)*

**Contoh Output:**

| nama_produk        | kategori   | nama_supplier      | kota      |
|--------------------|------------|--------------------| ----------|
| Laptop Core i7     | Elektronik | PT Maju Jaya       | Surabaya  |
| Mouse Wireless     | Aksesoris  | PT Maju Jaya       | Surabaya  |
| Kabel HDMI 2m      | Aksesoris  | CV Sinar Logistik  | Jakarta   |
| SSD 1TB            | Elektronik | Global Parts Ltd   | Singapore |
| Keyboard Mekanikal | Aksesoris  | CV Sinar Logistik  | Jakarta   |
| Monitor 24 inch    | Elektronik | PT Maju Jaya       | Surabaya  |
| Webcam HD          | Aksesoris  | NULL               | NULL      |
| NULL               | NULL       | PT Karya Mandiri   | Bandung   |


---