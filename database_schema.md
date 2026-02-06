# Desain Skema Database - RuangKita

Dokumen ini menjelaskan rancangan basis data yang digunakan untuk sistem peminjaman ruangan **RuangKita**.

## 1. Entity Relationship Diagram (Conceptual)
Sistem ini terdiri dari tiga entitas utama: **User**, **Room**, dan **Loan**.

---

## 2. Struktur Tabel (Data Dictionary)

### A. Tabel `Users`
Menyimpan data pengguna sistem (Mahasiswa dan Admin).

| Nama Kolom | Tipe Data | Deskripsi |
| :--- | :--- | :--- |
| `Id` | GUID / Int | Primary Key |
| `FullName` | Varchar(100) | Nama lengkap pengguna |
| `Email` | Varchar(50) | Email institusi (PENS) |
| `Role` | Varchar(20) | Admin atau Student |

### B. Tabel `Rooms`
Menyimpan daftar ruangan yang tersedia untuk dipinjam.

| Nama Kolom | Tipe Data | Deskripsi |
| :--- | :--- | :--- |
| `Id` | GUID / Int | Primary Key |
| `RoomName` | Varchar(50) | Contoh: "Lab Multimedia", "Ruang Kelas 1" |
| `Capacity` | Integer | Kapasitas maksimal orang |
| `Facility` | Text | Deskripsi fasilitas (AC, Proyektor, dll) |

### C. Tabel `Loans` (Peminjaman)
Menyimpan transaksi peminjaman ruangan.

| Nama Kolom | Tipe Data | Deskripsi |
| :--- | :--- | :--- |
| `Id` | GUID / Int | Primary Key |
| `UserId` | GUID / Int | Foreign Key ke tabel Users |
| `RoomId` | GUID / Int | Foreign Key ke tabel Rooms |
| `LoanDate` | Date | Tanggal peminjaman |
| `StartTime` | Time | Jam mulai |
| `EndTime` | Time | Jam selesai |
| `Status` | Varchar(20) | Pending, Approved, Rejected |

---

## 3. SQL Data Definition Language (DDL)
Berikut adalah cuplikan kode SQL untuk menginisialisasi tabel ini:

```sql
CREATE TABLE Users (
    Id SERIAL PRIMARY KEY,
    FullName VARCHAR(100) NOT NULL,
    Email VARCHAR(50) UNIQUE NOT NULL,
    Role VARCHAR(20) NOT NULL
);

CREATE TABLE Rooms (
    Id SERIAL PRIMARY KEY,
    RoomName VARCHAR(50) NOT NULL,
    Capacity INT,
    Facility TEXT
);

CREATE TABLE Loans (
    Id SERIAL PRIMARY KEY,
    UserId INT REFERENCES Users(Id),
    RoomId INT REFERENCES Rooms(Id),
    LoanDate DATE NOT NULL,
    Status VARCHAR(20) DEFAULT 'Pending'
);