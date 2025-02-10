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
FROM nasabah, pemasukkan
WHERE nasabah.No_Rekening = pemasukkan.No_Rekening;
```

---
