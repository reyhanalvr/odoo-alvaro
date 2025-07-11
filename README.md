# Server Odoo dengan implementasi HA

Dokumentasi ini merangkum penyiapan infrastruktur Odoo 14 CE dengan konfigurasi High Availability.

---

## 1. Arsitektur Server (Tugas 1)

Sistem ini terdiri dari 4 server dengan peran sebagai berikut:

| Nama Server | IP Publik | Peran |
| :--- | :--- | :--- |
| `web-server` | 54.252.158.57 | Load Balancer (Nginx) |
| `app-1` | 52.62.165.194 | Server Aplikasi Odoo 1 |
| `app-2` | 3.104.76.236 | Server Aplikasi Odoo 2 |
| `database` | 13.236.86.86 | Server Database PostgreSQL |

Untuk akses ke server sendiri menggunakan key yang disediakan di repository
---

## 2. Penyiapan Aplikasi & Addons (Tugas 2 & 3)

* **Odoo 14 CE** telah terinstal di server `app-1` dan `app-2`.
* Kedua server aplikasi terhubung ke satu database di server `database`.
* Repository addons kustom dari GitHub telah di-clone ke `/opt/odoo/odoo-alvaro` di kedua server aplikasi.
* File konfigurasi `odoo.conf` telah diperbarui dengan `addons_path` yang sesuai dan `proxy_mode = True`.

**Workflow Update Addons:**
1.  Push perubahan ke repository GitHub.
2.  Masuk ke direktori `/opt/odoo/odoo-alvaro` di setiap server aplikasi.
3.  Jalankan `git pull`.
4.  Restart layanan Odoo.

---

## 3. Skrip Otomatisasi (Tugas 4 & 5)

Dua skrip telah dibuat untuk mempermudah manajemen server.

### A. Restart Semua Layanan
* **Lokasi Skrip:** `/home/ubuntu/restart_server.sh` di server `web-server`.
* **Fungsi:** Merestart Nginx di `web-server` dan Odoo di `app-1` & `app-2`.
* **Cara Menjalankan:** `./restart_server.sh`

### B. Backup Database
* **Lokasi Skrip:** `/home/ubuntu/backup_db.sh` di server `database`.
* **Fungsi:** Membuat backup database `test` ke dalam file `.zip` dengan format nama berisi tanggal dan waktu.
* **Cara Menjalankan:** `./backup_db.sh` (akan meminta password user database).
DB Cred = P@ssw0rd

---

## 4. URL Akses (Deliverable 2)

Sistem dapat diakses melalui alamat IP publik dari Load Balancer Server:
**http://54.252.158.57**
