## 1. Menampilkan Data Pengeluaran

Query ini digunakan untuk menampilkan data pengeluaran dengan informasi nomor rekening, nama nasabah, jenis kelamin (dikonversi menjadi **LK** untuk laki-laki dan **PR** untuk perempuan), tanggal transaksi, serta saldo keluar.

```sql
SELECT pengeluaran.Id_Pengeluaran,
       nasabah.No_Rekening,
       nasabah.Nama_Nasabah,
       IF(nasabah.Jenis_Kelamin = 'Laki - Laki', 'LK', 'PR') AS "Jenis Kelamin",
       pengeluaran.Tanggal_Waktu,
       pengeluaran.Saldo_Keluar
FROM nasabah, pengeluaran
WHERE nasabah.No_Rekening = pengeluaran.No_Rekening;
```

### **Contoh Hasil Query**
| Id_Pengeluaran | No_Rekening | Nama_Nasabah    | Jenis Kelamin | Tanggal_Waktu        | Saldo_Keluar |
|---------------|------------|-----------------|--------------|----------------------|-------------|
| 001-01-0001  | 001-01-0001 | Bambang Susilo  | LK          | 2024-08-22 06:41:53 | 100000      |
| 001-01-0002  | 001-01-0002 | Mawar Melati    | PR          | 2024-08-22 06:43:30 | 200000      |
| 001-01-0005  | 001-01-0005 | Nurul Sintya    | PR          | 2024-08-22 06:43:32 | 250000      |
| 001-01-0006  | 001-01-0006 | Kenan Sudrajat  | LK          | 2024-08-22 06:42:33 | 350000      |

---

## 2. Menampilkan Total Saldo Masuk dan Hadiah

Query ini digunakan untuk menampilkan nomor rekening, nama nasabah, total saldo masuk, serta hadiah berdasarkan jumlah saldo yang masuk.  
Ketentuan hadiah:
- Jika total saldo masuk **1000000** → Hadiah: **Jam Dinding**
- Jika total saldo masuk **>=750000** → Hadiah: **Kaos**
- Jika total saldo masuk **<750000** → **Tidak Dapat Hadiah**

```sql
SELECT nasabah.No_Rekening,
       nasabah.Nama_Nasabah,
       SUM(pemasukkan.Saldo_Masuk) AS "Total Saldo Masuk",
       IF(SUM(pemasukkan.Saldo_Masuk) = 1000000, "Jam Dinding",
          IF(SUM(pemasukkan.Saldo_Masuk) >= 750000, "Kaos", "Tidak Dapat")) AS "Hadiah"
FROM pemasukkan, nasabah
WHERE pemasukkan.No_Rekening=nasabah.No_Rekening
GROUP BY nasabah.No_Rekening
```

---

## 1. Menampilkan Total Saldo Masuk dan Hadiah

Query ini digunakan untuk menampilkan nomor rekening, nama nasabah, total saldo masuk, serta hadiah berdasarkan jumlah saldo yang masuk.  
Ketentuan hadiah:
- Jika total saldo masuk **>=1000000** → Hadiah: **Jam Dinding**
- Jika total saldo masuk **<1000000** → **Tidak Dapat Hadiah**  
- Hanya menampilkan nasabah dengan saldo masuk minimal **300000**.

```sql
SELECT nasabah.No_Rekening,
       nasabah.Nama_Nasabah,
       SUM(pemasukkan.Saldo_Masuk) AS "Total Saldo Masuk",
       IF(SUM(pemasukkan.Saldo_Masuk) >= 1000000, "Jam Dinding", "Tidak Dapat") AS "Hadiah"
FROM pemasukkan, nasabah
WHERE pemasukkan.No_Rekening = nasabah.No_Rekening
GROUP BY nasabah.No_Rekening
HAVING SUM(pemasukkan.Saldo_Masuk) >= 300000;
```

### **Contoh Hasil Query**
| No_Rekening  | Nama_Nasabah   | Total Saldo Masuk | Hadiah       |
|-------------|---------------|------------------|-------------|
| 001-01-0001 | Bambang Susilo | 600000          | Tidak Dapat |
| 001-01-0003 | Bobi Nasution  | 750000          | Tidak Dapat |
| 001-01-0004 | Nurul Sintya   | 1000000         | Jam Dinding |
| 001-01-0005 | Kenan Sudrajat | 300000          | Tidak Dapat |

--- 

## 1. Menampilkan Nama Nasabah, Alamat, Kelurahan, dan Kode Kelurahan

Query ini digunakan untuk menampilkan informasi nasabah termasuk nama, alamat, kelurahan, kode kelurahan (berdasarkan kondisi tertentu), dan kota/kabupaten.

Ketentuan Kode Kelurahan:
- **Medono** → **M**
- **Kauman** → **KA**
- **Kutosari** → **KU**
- **Selain itu** → **H**

```sql
SELECT Nama_Nasabah,
       alamat,
       Kelurahan,
       CASE Kelurahan
            WHEN "Medono" THEN "M"
            WHEN "Kauman" THEN "KA"
            WHEN "Kutosari" THEN "KU"
            ELSE "H"
       END AS "Kode Kelurahan",
       Kota_Kab
FROM nasabah;
```

### **Contoh Hasil Query**
| Nama_Nasabah    | Alamat             | Kelurahan  | Kode Kelurahan | Kota_Kab             |
|----------------|--------------------|-----------|---------------|----------------------|
| Bambang Susilo | Jl. Melati No. 25   | Kauman    | KA            | Kota Pekalongan     |
| Mawar Melati   | Jl. Toba No. 20     | Kauman    | KA            | Kota Pekalongan     |
| Bobi Nasution  | Jl. Kenanga No. 1   | Medono    | M             | Kota Pekalongan     |
| Nurul Sintya   | Jl. Kenanga No. 5   | Medono    | M             | Kota Pekalongan     |
| Kenan Sudrajat | Jl. Melati No. 1    | Kutosari  | KU            | Kabupaten Pekalongan |
| Siti Nurbaya   | Jl. Anggrek No. 12  | Harjosari | H             | Kabupaten Pekalongan |
| Joko Susilo    | Jl. Kenanga No. 10  | Kutosari  | KU            | Kabupaten Pekalongan |

--- 

## 1. Menampilkan Total Saldo Masuk dan Hadiah

Query ini digunakan untuk menampilkan nomor rekening, nama nasabah, total saldo masuk, serta hadiah berdasarkan jumlah saldo masuk.

### **Ketentuan Hadiah:**
- Jika total saldo masuk **>=1000000** → Hadiah: **Jam Dinding**
- Jika total saldo masuk **<1000000** → **Tidak Dapat Hadiah**

```sql
SELECT nasabah.No_Rekening,
       nasabah.Nama_Nasabah,
       SUM(pemasukkan.Saldo_Masuk) AS "Total Saldo Masuk",
       CASE 
           WHEN SUM(pemasukkan.Saldo_Masuk) >= 1000000 THEN "Jam Dinding"
           ELSE "Tidak Dapat"
       END AS "Hadiah"
FROM pemasukkan, nasabah
WHERE pemasukkan.No_Rekening = nasabah.No_Rekening
GROUP BY nasabah.No_Rekening;
```

### **Contoh Hasil Query**
| No_Rekening  | Nama_Nasabah   | Total Saldo Masuk | Hadiah       |
|-------------|---------------|------------------|-------------|
| 001-01-0001 | Bambang Susilo | 600000          | Tidak Dapat |
| 001-01-0002 | Mawar Melati   | 15000           | Tidak Dapat |
| 001-01-0003 | Bobi Nasution  | 750000          | Tidak Dapat |
| 001-01-0004 | Nurul Sintya   | 1000000         | Jam Dinding |
| 001-01-0005 | Kenan Sudrajat | 300000          | Tidak Dapat |
| 001-01-0006 | Siti Nurbaya   | 50000           | Tidak Dapat |

---

## 1. Menampilkan Total Saldo Masuk dan Hadiah

Query ini digunakan untuk menampilkan nomor rekening, nama nasabah, total saldo masuk, serta hadiah berdasarkan jumlah saldo masuk.

### **Ketentuan Hadiah:**
- Jika total saldo masuk **>=1000000** → Hadiah: **Jam Dinding**
- Jika total saldo masuk **>750000** → Hadiah: **Kaos**
- Jika total saldo masuk **≤750000** → **Tidak Dapat Hadiah**

```sql
SELECT nasabah.No_Rekening,
       nasabah.Nama_Nasabah,
       SUM(pemasukkan.Saldo_Masuk) AS "Total Saldo Masuk",
       CASE 
           WHEN SUM(pemasukkan.Saldo_Masuk) >= 1000000 THEN "Jam Dinding"
           WHEN SUM(pemasukkan.Saldo_Masuk) > 750000 THEN "Kaos"
           ELSE "Tidak Dapat"
       END AS "Hadiah"
FROM pemasukkan, nasabah
WHERE pemasukkan.No_Rekening = nasabah.No_Rekening
GROUP BY nasabah.No_Rekening;
```

### **Contoh Hasil Query**
| No_Rekening  | Nama_Nasabah   | Total Saldo Masuk | Hadiah       |
|-------------|---------------|------------------|-------------|
| 001-01-0001 | Bambang Susilo | 600000          | Tidak Dapat |
| 001-01-0002 | Mawar Melati   | 15000           | Tidak Dapat |
| 001-01-0003 | Bobi Nasution  | 750000          | Kaos        |
| 001-01-0004 | Nurul Sintya   | 1000000         | Jam Dinding |
| 001-01-0005 | Kenan Sudrajat | 300000          | Tidak Dapat |
| 001-01-0006 | Siti Nurbaya   | 50000           | Tidak Dapat |

---
 
## 1. Menampilkan Total Saldo Masuk dan Hadiah

Query ini digunakan untuk menampilkan nomor rekening, nama nasabah, total saldo masuk, serta hadiah berdasarkan jumlah saldo masuk.

### **Ketentuan Hadiah:**
- Jika total saldo masuk **>=1000000** → Hadiah: **Jam Dinding**
- Jika total saldo masuk **>=750000** → Hadiah: **Kaos**
- Jika total saldo masuk **<750000** → **Tidak Dapat Hadiah**
- Hanya menampilkan nasabah dengan saldo masuk minimal **600000**.

```sql
SELECT nasabah.No_Rekening,
       nasabah.Nama_Nasabah,
       SUM(pemasukkan.Saldo_Masuk) AS "Total Saldo Masuk",
       CASE 
           WHEN SUM(pemasukkan.Saldo_Masuk) >= 1000000 THEN "Jam Dinding"
           WHEN SUM(pemasukkan.Saldo_Masuk) >= 750000 THEN "Kaos"
           ELSE "Tidak Dapat"
       END AS "Hadiah"
FROM pemasukkan, nasabah
WHERE pemasukkan.No_Rekening = nasabah.No_Rekening
GROUP BY nasabah.No_Rekening
HAVING SUM(pemasukkan.Saldo_Masuk) >= 600000;
```

### **Contoh Hasil Query**
| No_Rekening  | Nama_Nasabah   | Total Saldo Masuk | Hadiah       |
|-------------|---------------|------------------|-------------|
| 001-01-0001 | Bambang Susilo | 600000          | Tidak Dapat |
| 001-01-0003 | Bobi Nasution  | 750000          | Kaos        |
| 001-01-0004 | Nurul Sintya   | 1000000         | Jam Dinding |

---

### TRIGER
 

## **1. Pengenalan Trigger**
**Trigger** dalam SQL adalah mekanisme yang memungkinkan eksekusi otomatis perintah tertentu ketika terjadi perubahan pada tabel database (INSERT, UPDATE, DELETE).

### **Keuntungan Menggunakan Trigger**
1. **Mendeteksi Perubahan Data** → Memudahkan pencatatan perubahan data, siapa yang melakukan, dan kapan perubahan terjadi.
2. **Otomatisasi Proses** → Dapat memberikan sinyal atau mengeksekusi program lain secara otomatis.
3. **Meningkatkan Keamanan & Integritas Data** → Memastikan aturan tertentu diterapkan sebelum atau sesudah operasi pada tabel.

---

## **2. Jenis-Jenis Trigger**
### **a. AFTER Trigger**
Trigger yang dieksekusi **setelah** operasi **INSERT, UPDATE, atau DELETE** pada tabel.

### **b. BEFORE Trigger**
Trigger yang dieksekusi **sebelum** operasi **INSERT, UPDATE, atau DELETE** pada tabel.

| **Waktu Trigger**      | **Deskripsi** |
|-----------------|----------------------------------|
| **BEFORE INSERT** | Dieksekusi sebelum record dimasukkan ke database. |
| **AFTER INSERT** | Dieksekusi setelah record dimasukkan ke database. |
| **BEFORE UPDATE** | Dieksekusi sebelum record diperbarui di database. |
| **AFTER UPDATE** | Dieksekusi setelah record diperbarui di database. |
| **BEFORE DELETE** | Dieksekusi sebelum record dihapus di database. |
| **AFTER DELETE** | Dieksekusi setelah record dihapus di database. |

---

## **3. Cara Membuat Trigger**
### **Sintaks Dasar**
```sql
CREATE TRIGGER trigger_name 
trigger_time trigger_event 
ON tbl_name 
FOR EACH ROW 
trigger_stmt;
```
**Keterangan:**
- `trigger_name` → Nama trigger.
- `trigger_time` → Waktu trigger (**BEFORE** atau **AFTER**).
- `trigger_event` → Jenis operasi yang memicu trigger (**INSERT, UPDATE, DELETE**).
- `tbl_name` → Nama tabel yang terkait.
- `trigger_stmt` → Perintah SQL yang akan dijalankan saat trigger aktif.

---

## **4. Contoh Implementasi Trigger**
### **a. Trigger untuk INSERT (Menambah Data)**
```sql
CREATE TRIGGER INSERT_Nasabah 
AFTER INSERT ON nasabah 
FOR EACH ROW 
INSERT INTO log_nasabah VALUES ('Tambah Data', NOW());
```
- **Trigger ini akan mencatat setiap penambahan data baru pada tabel `nasabah` ke dalam `log_nasabah`.**

### **b. Trigger untuk UPDATE (Mengubah Data)**
```sql
CREATE TRIGGER UPDATE_Nasabah 
AFTER UPDATE ON nasabah 
FOR EACH ROW 
INSERT INTO log_nasabah VALUES ('Ubah Data', NOW());
```
- **Setiap perubahan data pada tabel `nasabah`, akan tercatat di tabel `log_nasabah`.**

### **c. Trigger untuk DELETE (Menghapus Data)**
```sql
CREATE TRIGGER DELETE_Nasabah 
AFTER DELETE ON nasabah 
FOR EACH ROW 
INSERT INTO log_nasabah VALUES ('Hapus Data', NOW());
```
- **Trigger ini akan mencatat setiap penghapusan data dari tabel `nasabah` ke dalam `log_nasabah`.**

---

## **5. Langkah Praktikum**
1. **Buat Tabel Log Nasabah**
```sql
CREATE TABLE log_nasabah (
    aksi VARCHAR(20),
    waktu TIMESTAMP
);
```
2. **Buat Trigger untuk INSERT, UPDATE, dan DELETE** seperti contoh di atas.
3. **Lakukan operasi INSERT, UPDATE, dan DELETE pada tabel `nasabah`**.
4. **Periksa tabel `log_nasabah` untuk melihat apakah perubahan sudah tercatat**.

---

## **6. Tes Formatif**
Latihan untuk memahami konsep trigger:
1. **Buat database latihan** `latihantrigger1_xxxx`.
2. **Buat tabel pegawai** `pegawaiXXXX`.
3. **Isikan 3 data awal** pada tabel `pegawaiXXXX`.
4. **Buat tabel log_pegawaiXXXX** untuk mencatat perubahan.
5. **Buat trigger untuk INSERT, UPDATE, dan DELETE** pada `pegawaiXXXX`.
6. **Lakukan operasi pada `pegawaiXXXX`** dan periksa perubahan di `log_pegawaiXXXX`.

---

## **7. Kesimpulan**
Trigger dalam database berfungsi sebagai pemicu otomatis ketika terjadi perubahan data pada tabel. Dengan menggunakan **AFTER** dan **BEFORE TRIGGER**, kita dapat mengontrol dan mencatat setiap perubahan dalam database secara otomatis.

---

## **Referensi**
- Kristiaono, P. (2015). *Pemrograman Stored Procedure pada MySQL*. Yogyakarta: Andi Offset.

---
 
