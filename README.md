# Uji Penetrasi pada Sistem Kendali Pintu Pintar Berbasis Mikrokontroler: Analisis Dampak Serangan DDoS Terhadap Stabilitas Koneksi.

---

##  Anggota Kelompok 

| No | Nama | NIM |
|----|------|-----|
| 1. | **Angelia Sari Hotma** | 09030282327020 |
| 2. | **Anis Levina** | 09030282327038 |
| 3. | **Fathur Rahman** | 09030282327047 |
| 4. | **Muhammad Daffa** | 09030282327029 |
| 5. | **Nabila Asyura Chandra Putri** | 09030582327098 |
| 6. | **Nurhaliza** | 09030282327062 |

---

Sistem keamanan pintu konvensional memiliki kelemahan fatal seperti kunci yang mudah diduplikasi. Proyek **Smart Door Lock** ini mengatasi hal tersebut menggunakan autentikasi **RFID dan Keypad**. Namun, karena perangkat ini terhubung ke jaringan (IoT) untuk pemantauan, terdapat risiko serangan siber yang menargetkan ketersediaan layanan (*Availability*).

**Tujuan Pengujian:**
1.   Mengidentifikasi alamat IP perangkat Smart Door Lock di jaringan lokal.
2.   Menguji ketahanan mikrokontroler (ESP32) terhadap serangan banjir paket (*Packet Flooding*).
3.   Membuktikan bahwa serangan jaringan dapat menyebabkan perangkat mengalami **RTO (Request Time Out)** dan kehilangan koneksi.

---

##  Deskripsi Perangkat (Smart Door Lock)
Sistem ini menggunakan mikrokontroler ESP32 sebagai otak utama yang mengatur sensor dan aktuator.

### 1. Cara Kerja Sistem
* **Input:** User menempelkan kartu RFID atau memasukkan PIN pada Keypad.
* **Proses:** ESP32 memvalidasi data. Jika benar, Servo bergerak membuka pintu.
* **Output:** Status ditampilkan di LCD dan data akses dikirim ke server/monitoring.
* **Kelemahan:** Koneksi Wi-Fi pada mikrokontroler rentan terhadap beban trafik tinggi.
---
### 2. Skematik dan projek
Berikut adalah rancangan perangkat keras 

<img width="500" height="500" alt="image" src="https://github.com/user-attachments/assets/9f02431b-d100-4ea6-bf14-9ff9096d30b7" /> <img width="300" height="300" alt="image" src="https://github.com/user-attachments/assets/64c655ec-99e1-44c5-8b06-d9d8e8bb9bb0" />


**Komponen Utama:**
* **Mikrokontroler:** NodeMCU (ESP32).
* **Sensor:** RFID & Keypad 4x4.
* **Output:** Servo, LCD 16x2, Buzzer.
---
## 3 Target Pengujian
Target serangan adalah antarmuka jaringan nirkabel pada mikrokontroler yang menghubungkan alat dengan internet.

* **Perangkat Target:** NodeMCU ESP32.
* **Layanan Terkait:** Koneksi Wi-Fi (802.11) 
* **Vektor Serangan:** TCP/ICMP Flooding pada Port terbuka.
---
##  Langkah-Langkah Pengujian Yang Pertama (pakai ssid wifinya lgsg dengan cara menangkap sinyal wifi)  
### 1. Persiapan & Pengecekan Adapter WiFi (*Network Interface Setup*)

<img width="500" height="500" alt="image" src="https://github.com/user-attachments/assets/d39a5f52-3698-4edf-8f6a-296c7751f957" />

Tahap awal ini bertujuan untuk memastikan bahwa adapter WiFi pada komputer penyerang (Kali Linux) sudah terdeteksi, aktif, dan mampu menangkap sinyal dari jaringan target.

Langkah pertama adalah memeriksa apakah sistem operasi mendeteksi perangkat keras kartu jaringan nirkabel
Perintah ini menampilkan daftar antarmuka jaringan. Pastikan muncul interface bernama wlan0, yang menandakan adapter WiFi siap digunakan.
---


### 2. Membuat Script Ddos Untuk membanjiri lalu lintas jaringan
<img width="500" height="160" alt="image" src="https://github.com/user-attachments/assets/dfc57004-97f3-4ac0-b6c4-13621b98ac79" />

 Gambar ini menampilkan aktivitas pemrograman di dalam terminal Linux, di mana pengguna bernama Fathur sedang membuat alat untuk serangan jaringan. Pada baris pertama, pengguna mengetikkan perintah nano wifidos.py yang bertujuan membuka editor teks Nano untuk menulis skrip bahasa pemrograman Python bernama "wifidos". Nama berkas ini mengindikasikan bahwa kode tersebut dirancang untuk melakukan WiFi Denial of Service (DoS), yaitu sebuah metode serangan siber untuk memutus koneksi internet orang lain dengan cara membanjiri lalu lintas jaringan WiFi tersebut. 

Setelah selesai, pengguna mengetikkan perintah ls pada baris berikutnya untuk memeriksa isi direktori atau folder tempat ia bekerja. Hasil dari perintah tersebut terlihat di bagian paling bawah layar, yang menampilkan daftar folder standar seperti Desktop dan Documents, serta membuktikan bahwa berkas wifidos.py telah berhasil dibuat dan tersimpan di dalam sistem. Intinya, gambar ini merekam momen ketika seseorang sedang menyiapkan kode program yang berfungsi untuk mengganggu stabilitas jaringan nirkabel.
---
### 3. Isi Penulisan Script 
<img width="500" height="500" alt="image" src="https://github.com/user-attachments/assets/f89ff0a2-fd8d-44f7-9933-e0c1367d7840" />
Bagian kode yang terlihat di layar ini fokus pada persiapan sistem dan manajemen file sebelum serangan jaringan yang sebenarnya dilakukan. 

Pertama, script ini memiliki fitur keamanan untuk memastikan pengguna menjalankan program dengan hak akses tertinggi atau Super User (Sudo). Kode akan mengecek sistem, dan jika program dijalankan tanpa sudo, maka script akan otomatis berhenti. Hal ini wajib dilakukan karena mengakses dan memanipulasi kartu jaringan (network card) untuk keperluan hacking membutuhkan izin penuh dari sistem operasi.

Kedua, script ini menjalankan logika "pembersihan" otomatis sebelum mulai bekerja. Program akan memindai folder untuk mencari file berekstensi .csv (biasanya berisi data hasil scan jaringan sebelumnya). Jika ditemukan file lama, script tidak akan menimpanya, melainkan membuat folder baru bernama "backup" dan memindahkan file-file lama tersebut ke sana dengan penanda waktu (timestamp). Tujuannya agar data hasil peretasan yang baru tidak tercampur dengan data yang lama, sehingga hasilnya lebih rapi dan terorganisir.

ini isi script dari gambar tersebut
[wifidos.py](https://github.com/user-attachments/files/24058844/wifidos.py)

---

### 4: Eksekusi Script & Pemilihan Interface
<img width="500" height="500" alt="image" src="https://github.com/user-attachments/assets/dd09c8b5-e2ac-4601-b711-6290eba1f1e7" />

Jalankan script menggunakan perintah `sudo` dan `python3`. Pada tahap ini, script akan memindai sistem untuk mencari *wireless adapter* yang kompatibel.

Seperti yang terlihat pada output, script berhasil mendeteksi adapter **wlan0**. Adapter ini akan digunakan untuk memindai sinyal WiFi di sekitar dan melakukan injeksi paket (serangan) ke target yang spesifik.

**Perintah:**
```bash
sudo python3 wifidos.py
```
output terminal
```bash
****************************************************************
* Copyright of David Bombal, 2021                              *
...
****************************************************************
The following WiFi interfaces are available:
0 - wlan0
Please select the interface you want to use for the attack: 0

WiFi adapter connected!
Now let's kill conflicting processes:

Killing these processes:
  PID Name
  1794 wpa_supplicant

Putting Wifi adapter into monitored mode:
PHY   Interface   Driver      Chipset
phy0  wlan0       ath9k_htc   Qualcomm Atheros Communications AR9271 802.11n
```
Penjelasan Proses:

1. Deteksi Interface: Script membaca adapter wlan0.
2. Kill Process: Script mematikan proses latar belakang yang mengganggu (seperti wpa_supplicant) agar adapter bisa masuk ke mode monitor tanpa gangguan.
3. Monitor Mode: Adapter diubah dari mode Managed (biasa) ke Monitor Mode agar bisa menangkap semua lalu lintas WiFi di udara.
---
## 5. Memilih Target & Melancarkan Serangan (DeAuth)
<img width="500" height="500" alt="image" src="https://github.com/user-attachments/assets/ef2375eb-d6c7-43a5-bf78-d30b157c4594" />
Setelah proses inisialisasi selesai, script akan otomatis memindai jaringan WiFi (Access Point) yang berada dalam jangkauan. Anda akan melihat daftar yang berisi:
* **No:** Nomor urut untuk memilih target.
* **BSSID:** Alamat MAC dari router/WiFi target.
* **Channel:** Saluran frekuensi yang digunakan.
* **ESSID:** Nama WiFi yang muncul di HP/Laptop.

**Instruksi:**
1.  Tunggu hingga nama WiFi target Anda muncul di daftar.
2.  Tekan `Ctrl+C` pada keyboard untuk menghentikan pemindaian dan masuk ke menu pemilihan.
3.  Masukkan **Nomor** yang sesuai dengan target (Contoh pada gambar: `1` untuk "Robinson Family").
4.  Tekan **Enter**.

**Output Terminal:**
```output
Scanning. Press Ctrl+C when you want to select which wireless network you want to attack.

No |    BSSID              |    Channel|    ESSID
___|    ___________________|    _______|    ______________________________
0       FC:A6:CD:98:60:B8       11          Putrowayang
1       8C:DE:F9:0E:28:12       11          Robinson Family
2       00:E0:20:23:EF:4B       11          Putrowayang_Ext
3       D4:CA:6D:FB:AF:C5       1           KRU
4       9C:88:AD:0C:54:A1       4           wifi.id@home

Ready to make choice.
Please select a choice from above: 1

17:48:22  Waiting for beacon frame (BSSID: 8C:DE:F9:0E:28:12) on channel 11
17:48:23  Sending DeAuth (code 7) to broadcast -- BSSID: [8C:DE:F9:0E:28:12]
17:48:23  Sending DeAuth (code 7) to broadcast -- BSSID: [8C:DE:F9:0E:28:12]
17:48:27  Sending DeAuth (code 7) to broadcast -- BSSID: [8C:DE:F9:0E:28:12]
...
```
1. Flooding/DeAuth: Saat muncul tulisan Sending DeAuth, script sedang membanjiri target dengan paket Deauthentication. Ini memaksa perangkat yang terhubung ke WiFi tersebut untuk putus (disconnect) secara terus-menerus.
2. Code 7: Ini adalah kode status WiFi yang berarti "Class 3 frame received from nonassociated station", yang digunakan untuk menipu router agar memutus koneksi klien.
---
## 6. Verifikasi Hasil Serangan (Ping Test)
<img width="500" height="500" alt="image" src="https://github.com/user-attachments/assets/23c08b1c-63c4-48a0-9272-e2a89199f576" />

Untuk memastikan apakah serangan Deauthentication berhasil, lakukan pengujian koneksi internet pada perangkat target (atau perangkat uji coba yang terhubung ke WiFi yang sama).

Gunakan perintah `ping` ke server yang stabil seperti Google.

**Perintah (CMD Windows):**
```cmd
ping google.com -t
```
1. Sebelum Serangan: Terlihat respon Reply from..., menandakan internet lancar.
2. Saat Serangan Berjalan: Muncul pesan Request timed out. atau Destination host unreachable.
3. Kesimpulan: Paket DeAuth berhasil memaksa perangkat target terputus dari Router/Access Point, sehingga akses internet lumpuh total.
---
##  Langkah-Langkah Pengujian Yang Kedua (pakai ip yg terhubung ke alatnya)  
### 1. Mencari alamat ip jaringan pada alat
<img width="500" height="480" alt="image" src="https://github.com/user-attachments/assets/d20074ed-fe3b-4677-a69f-977419cc91a2" />

Pada langkah pengujian keduaini , pengujian dilakukan dengan menargetkan alamat IP dari Smart Door Lock (192.168.1.22). Tujuan dari pengujian ini adalah untuk mengevaluasi stabilitas mikrokontroler perangkat ketika menerima lalu lintas jaringan yang tidak wajar
---
### 2. Menginstal paket git
<img width="500" height="500" alt="image" src="https://github.com/user-attachments/assets/729a59df-83a6-46b8-81cb-e21085ca4657" />

Git wajib diinstal agar sistem bisa terhubung dan mengunduh kode program yang ada di situs GitHub. Tanpa Git, kita tidak bisa melakukan perintah cloning.Setelah Git terpasang, langkah selanjutnya mengunduh alat pengujian yang bernama DDoS-Ripper. dengan perintah
```bash
git clone https://github.com/palahsu/DDoS-Ripper.git
```
fungsi ini mengambil salinan kode program dari server GitHub milik pengembang ke komputer lokal kita. Setelah unduhan selesai, kita harus masuk ke dalam folder alat tersebut untuk memastikan siap digunakan. 
'cd DDoS-Ripper' Perintah untuk berpindah masuk ke dalam folder hasil unduhan tadi.
---
### 3. Eksekusi Script & Pemilihan Parameter Serangan
<img width="500" height="500" alt="image" src="https://github.com/user-attachments/assets/279f7ff2-7744-40e3-983e-c3506d4e064c" />

setelah proses cloning selesai, tahap selanjutnya adalah menjalankan program utama DRipper.py. Dari daftar parameter yang tersedia, kita memilih opsi -s (Server IP) opsi ini digunakan untuk menargetkan alamat IP spesifik. Ini sesuai dengan pengujian kita di mana targetnya adalah Smart Door Lock dengan IP 192.168.1.22. perintah bawah  untuk memulai serangan ke Smart Door Lock kita adalah:

```Bash
python3 DRipper.py -s 192.168.1.22 -t 135
```
---
### 4. Monitoring Proses Serangan (Packet Flooding)
Setelah perintah eksekusi dijalankan dengan parameter IP target, alat DDoS-Ripper akan segera memulai proses pengiriman paket data secara massal.
<img width="500" height="500" alt="image" src="https://github.com/user-attachments/assets/b216b110-4566-4c61-a775-a2b26b8a9df5" />

analisis output terminal 
1. 192.168.1.22: Memverifikasi bahwa serangan terkunci pada IP Smart Door Lock.
2. port 80: Menunjukkan serangan diarahkan ke port web server (HTTP) pada perangkat.
3. turbo: 135: Menunjukkan tingkat kecepatan serangan yang digunakan.

Log Aktivitas:
1. Pesan packet sent! rippering yang muncul berulang kali dengan sangat cepat menunjukkan bahwa script berhasil mengirimkan beban data ke target tanpa henti.
2. Mon Dec 8 ...: Stempel waktu (timestamp) membuktikan bahwa serangan dilakukan secara real-time.

Indikator Keberhasilan Tahap Ini: Jika pesan ini terus mengalir tanpa henti dan tidak ada pesan error (seperti "Connection Refused"), berarti alat penyerang memiliki jalur koneksi yang terbuka ke Smart Door Lock dan sedang membanjiri sumber daya jaringannya.
---
### 5. Verifikasi Hasil Serangan
Setelah berhasil di run otomatis alat yg terhubung ke wifi dengan ip tersebut akan mengalami RTO.
<img width="500" height="514" alt="image" src="https://github.com/user-attachments/assets/b661e91a-1723-4e8e-a092-6fc850ff0552" />
Seperti yang terlihat pada gambar hasil pengujian, terjadi perubahan drastis pada respons perangkat saat serangan dimulai.

1. Kondisi Sebelum Serangan (Normal) Pada baris-baris awal, Smart Door Lock merespons dengan normal: Reply from 192.168.1.22: bytes=32 time=8ms TTL=64 Ini menandakan perangkat terhubung ke jaringan dan CPU mikrokontroler mampu memproses perintah standar.
2. Kondisi Saat Serangan Berlangsung (Down) Sesaat setelah script DDoS-Ripper dijalankan dan membanjiri target dengan paket data, respons berubah menjadi: Request timed out.

Analisis Teknis: Fenomena Request Timed Out (RTO) ini terjadi karena mikrokontroler pada Smart Door Lock mengalami Resource Exhaustion (Kelelahan Sumber Daya).
1. CPU Overload: Prosesor perangkat terlalu sibuk menangani ribuan paket sampah yang dikirimkan oleh alat penyerang.
2. Network Congestion: Antrian data pada kartu jaringan (network stack) perangkat penuh.

Akibatnya, perangkat tidak lagi memiliki sisa kapasitas untuk membalas paket ping yang sah, apalagi merespons perintah "Buka Pintu" dari alat pengguna.
---
## Kesimpulan
Berdasarkan pengujian ini, dapat disimpulkan bahwa Smart Door Lock dengan IP 192.168.1.22 rentan terhadap serangan Denial of Service (DoS) berbasis IP Flooding. Alat DDoS-Ripper berhasil melumpuhkan koneksi perangkat hanya dalam hitungan detik, menyebabkan perangkat gagal berfungsi (gagal merespons perintah) selama serangan berlangsung.

