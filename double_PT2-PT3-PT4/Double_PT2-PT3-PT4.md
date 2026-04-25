# Double Posttest 4

> **Mata Kuliah:** Basis Data  
> **Topik:** INSERT · SELECT · UPDATE · DELETE  
> **Database:** Sistem Supply Chain

---

## Skema Database

### Tabel `supplier`
```sql
CREATE TABLE supplier (
    id_supplier  INT AUTO_INCREMENT PRIMARY KEY,
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
    id_pelanggan  INT AUTO_INCREMENT PRIMARY KEY,
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

---

## 📥 Data Awal

```sql
INSERT INTO supplier (nama_supplier, kota, negara, no_telepon) VALUES
('PT Maju Jaya',       'Surabaya',  'Indonesia', '031-5551234'),
('CV Sinar Logistik',  'Jakarta',   'Indonesia', '021-7778899'),
('Global Parts Ltd',   'Singapore', 'Singapore', '+65-62345678'),
('PT Karya Mandiri',   'Bandung',   'Indonesia', '022-4443322');

INSERT INTO produk (nama_produk, kategori, harga_satuan, stok, id_supplier) VALUES
('Laptop Core i7',    'Elektronik', 12500000,  50, 1),
('Mouse Wireless',    'Aksesoris',    150000, 200, 1),
('Kabel HDMI 2m',     'Aksesoris',     75000, 300, 2),
('SSD 1TB',           'Elektronik',  1200000,  80, 3),
('Keyboard Mekanikal','Aksesoris',    850000, 120, 2),
('Monitor 24 inch',   'Elektronik',  2800000,  40, 1),
('Webcam HD',         'Aksesoris',    450000,  90, NULL);

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

Tambahkan data supplier baru ke tabel `supplier` dengan detail berikut menggunakan metode INSERT yang **menyebutkan nama kolom secara eksplisit**:

- Nama Supplier : `PT Nusantara Global`
- Kota          : `Semarang`
- Negara        : `Indonesia`
- No. Telepon   : `024-3334455`

**Note** *(setelah INSERT, lakukan `SELECT * FROM supplier`)*:

| id_supplier | nama_supplier       | kota      | negara    | no_telepon    |
|:-----------:|---------------------|-----------|-----------|---------------|
| 1           | PT Maju Jaya        | Surabaya  | Indonesia | 031-5551234   |
| 2           | CV Sinar Logistik   | Jakarta   | Indonesia | 021-7778899   |
| 3           | Global Parts Ltd    | Singapore | Singapore | +65-62345678  |
| 4           | PT Karya Mandiri    | Bandung   | Indonesia | 022-4443322   |
| **5**       | **PT Nusantara Global** | **Semarang** | **Indonesia** | **024-3334455** |



---

### Soal 2 

Masukkan **3 data produk sekaligus** ke tabel `produk` dalam **satu perintah INSERT** tanpa menyebutkan nama kolom:

- `(8, 'USB Hub 4 Port', 'Aksesoris', 120000, 150, 2)`
- `(9, 'RAM DDR4 16GB', 'Elektronik', 650000, 60, 3)`
- `(10, 'Headset Gaming', 'Aksesoris', 380000, 75, 4)`

**Note** *(setelah INSERT, tampilkan 3 data terakhir)*:

| id_produk | nama_produk    | kategori   | harga_satuan | stok | id_supplier |
|:---------:|----------------|------------|-------------:|:----:|:-----------:|
| 8         | USB Hub 4 Port | Aksesoris  |      120000  | 150  | 2           |
| 9         | RAM DDR4 16GB  | Elektronik |      650000  |  60  | 3           |
| 10        | Headset Gaming | Aksesoris  |      380000  |  75  | 4           |


---

### Soal 3 

Tampilkan **semua kolom** dari tabel `produk` yang memiliki **stok kurang dari 100 unit**.  

**Contoh Output:**

| id_produk | nama_produk    | kategori   | harga_satuan | stok | id_supplier |
|:---------:|----------------|------------|-------------:|:----:|:-----------:|
| 1         | Laptop Core i7 | Elektronik |  12500000    |  50  | 1           |
| 4         | SSD 1TB        | Elektronik |   1200000    |  80  | 3           |
| 6         | Monitor 24 inch| Elektronik |   2800000    |  40  | 1           |
| 7         | Webcam HD      | Aksesoris  |     450000   |  90  | NULL        |


---

### Soal 4 

Ubah **status** pesanan dengan `id_pesanan = 3` menjadi `'Dikirim'` dan perbarui `tanggal_pesanan` menjadi `'2024-02-05'`.  
Pastikan hanya baris dengan `id_pesanan = 3` yang berubah.

**Contoh Output** *(setelah UPDATE, lakukan `SELECT * FROM pesanan`)*:

| id_pesanan | id_pelanggan | tanggal_pesanan | status     | total_harga |
|:----------:|:------------:|-----------------|------------|------------:|
| 1          | 1            | 2024-01-10      | Selesai    | 12650000    |
| 2          | 2            | 2024-01-15      | Dikirim    |  1275000    |
| **3**      | **3**        | **2024-02-05**  | **Dikirim**|   **925000**|
| 4          | 1            | 2024-02-10      | Selesai    |    150000   |
| 5          | 2            | 2024-03-05      | Dibatalkan |  1200000    |
| 6          | 4            | 2024-03-15      | Dikirim    |  3250000    |



---

### Soal 5 

Hapus semua data pesanan dari tabel `pesanan` yang berstatus **`'Dibatalkan'`**.  
Gunakan `WHERE` agar hanya baris tersebut yang terhapus.

**Contoh Output** *(setelah DELETE, lakukan `SELECT * FROM pesanan`)*:

| id_pesanan | id_pelanggan | tanggal_pesanan | status   | total_harga |
|:----------:|:------------:|-----------------|----------|------------:|
| 1          | 1            | 2024-01-10      | Selesai  | 12650000    |
| 2          | 2            | 2024-01-15      | Dikirim  |  1275000    |
| 3          | 3            | 2024-02-01      | Diproses |   925000    |
| 4          | 1            | 2024-02-10      | Selesai  |   150000    |
| 6          | 4            | 2024-03-15      | Dikirim  |  3250000    |



---
