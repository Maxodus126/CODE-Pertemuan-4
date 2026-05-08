# Filesystem Storage and Virtual Disk Setup

## Deskripsi Proyek
Proyek ini bertujuan untuk mengelola dan memantau kapasitas filesystem, membuat virtual disk menggunakan file image, serta melakukan pengaturan struktur direktori dan file dengan hak akses yang berbeda-beda. Semua langkah dilakukan di lingkungan Linux dengan penggunaan perintah Bash dan utilitas seperti `df`, `du`, `losetup`, `fdisk`, dan `mount`.

---

## Fitur Utama

1. **Monitoring Disk dan Inode**
   - Mengecek kapasitas partisi (`df -h`) dan tipe filesystem (`df -Th`)
   - Memeriksa penggunaan inode (`df -i` dan `df -ih`)
   - Menampilkan direktori terbesar di filesystem menggunakan `du` dan `sort`
   - Memfilter filesystem tertentu dan menampilkan ringkasan kapasitas

2. **Manajemen File dan Direktori**
   - Membuat direktori `security` dengan sub-direktori `public`, `private`, `shared`, dan `scripts`
   - Membuat file contoh di setiap sub-direktori (`readme.txt`, `secret.txt`, `data.csv`, `greet.sh`)
   - Mengatur hak akses berbeda untuk tiap sub-direktori dan file (`chmod 755`, `chmod 700`, `chmod 775`, `chmod 644`, `chmod 600`)

3. **Pembuatan Virtual Disk**
   - Membuat file image `virtual_disk.img` dengan ukuran 512 MB
   - Menghubungkan file image ke loop device (`losetup`)
   - Membuat tabel partisi GPT dengan beberapa partisi:
     - Partisi 1: EFI System (50 MB)
     - Partisi 2: Linux Swap (100 MB)
     - Partisi 3: Linux filesystem (361 MB)
   - Membuat filesystem pada partisi:
     - Partisi 1: FAT32
     - Partisi 2: swap
     - Partisi 3: ext4 (label `DATA_VOL`)
   - Mount partisi data (`loop12p3`) ke `/mnt/vdisk-data`

4. **Pengisian Data Virtual Disk**
   - Membuat struktur direktori `project/{src,docs,tests}`
   - Mengisi data acak menggunakan `dd`:
     - `bigfile.bin` 50 MB
     - `src/app.bin` 20 MB
   - Membuat file `project/docs/readme.txt` berisi `README`
   - Mengecek kapasitas dan penggunaan inode pada virtual disk

5. **Manajemen Mount Point**
   - Membuat dan mount temporary filesystem (`tmpfs`) untuk percobaan
   - Menggunakan mount bind (`mount --bind`) untuk menyalin struktur `/etc` ke `/mnt/test-bind`
   - Unmount filesystem dan melepaskan loop device setelah selesai

6. **Hak Akses dan Keamanan**
   - File publik: dapat dibaca semua (`chmod 644`)
   - File privat: hanya user owner yang dapat mengakses (`chmod 600`)
   - Direktori publik dan scripts dapat diakses dan dieksekusi sesuai kebutuhan
   - Direktori private dilindungi (`chmod 700`) untuk keamanan data sensitif

---

## Struktur Direktori Akhir

praktikum-p4/
├─ virtual_disk.img
├─ security/
│ ├─ private/secret.txt
│ ├─ public/readme.txt
│ ├─ shared/data.csv
│ └─ scripts/greet.sh
└─ vdisk-mount/
└─ project/
├─ bigfile.bin
├─ docs/readme.txt
├─ src/app.bin
└─ tests/


---

## Utilitas dan Perintah yang Digunakan
- `df`, `du` → monitoring kapasitas disk
- `find`, `sort`, `head` → menemukan direktori terbesar
- `touch`, `mkdir`, `echo` → membuat file dan direktori
- `chmod`, `chown` → mengatur hak akses
- `losetup`, `fdisk`, `mkfs`, `mkswap`, `mount`, `partprobe` → membuat dan memanipulasi virtual disk
- `dd` → membuat file dummy untuk mengisi virtual disk

---

## Tujuan dan Manfaat
- Melatih manajemen filesystem dan virtual disk di Linux
- Memahami struktur direktori, hak akses, dan mounting
- Menyediakan simulasi storage untuk percobaan file dan project
- Menyiapkan lingkungan yang aman untuk pengelolaan data publik, privat, dan bersama