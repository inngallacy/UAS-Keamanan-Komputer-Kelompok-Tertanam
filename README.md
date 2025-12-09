# Uji Penetrasi Sistem Kendali Pintu Pintar Berbasis Mikrokontroler

## Analisis Dampak Serangan DDoS Terhadap Stabilitas Koneksi

---

## **Anggota Kelompok**

| No | Nama                            | NIM            |
| -- | ------------------------------- | -------------- |
| 1. | **Angelia Sari Hotma**          | 09030282327020 |
| 2. | **Anis Levina**                 | 09030282327038 |
| 3. | **Fathur Rahman**               | 09030282327047 |
| 4. | **Muhammad Daffa**              | 09030282327029 |
| 5. | **Nabila Asyura Chandra Putri** | 09030582327098 |
| 6. | **Nurhaliza**                   | 09030282327062 |

---

## **Pendahuluan**

Sistem keamanan pintu konvensional memiliki kelemahan seperti kunci yang mudah diduplikasi. Untuk mengatasi hal tersebut, proyek **Smart Door Lock** dikembangkan menggunakan autentikasi **RFID** dan **Keypad**. Namun, karena sistem ini terhubung ke jaringan (IoT), perangkat dapat menjadi target serangan siber, terutama serangan **Denial of Service (DoS)** yang mengancam aspek **Availability**.

---

## **Tujuan Pengujian**

1. Mengidentifikasi alamat IP Smart Door Lock pada jaringan lokal.
2. Menguji ketahanan mikrokontroler ESP32 terhadap serangan *Packet Flooding*.
3. Membuktikan bahwa serangan dapat menyebabkan **Request Timed Out (RTO)** dan hilangnya koneksi.

---


### Apa itu DoS?
**Denial of Service (DoS)** adalah serangan yang bertujuan melumpuhkan layanan agar tidak bisa diakses oleh pengguna yang sah. Bayangkan sebuah pintu masuk gedung yang hanya muat untuk satu orang. Jika kita mengirim ribuan "orang palsu" untuk berdesak-desakan di pintu tersebut secara bersamaan, maka orang asli (pengguna sah) tidak akan bisa masuk. Dalam pengujian ini, kami menggunakan metode **TCP SYN Flood**. Kami membanjiri ESP32 dengan permintaan koneksi palsu hingga memori/CPU-nya penuh (*Resource Exhaustion*), menyebabkan sistem melambat, *hang*, atau *reboot*.


---


## **Deskripsi Perangkat (Smart Door Lock)**

Sistem ini menggunakan **ESP32** sebagai mikrokontroler utama.

### **1. Cara Kerja Sistem**

* **Input:** RFID atau PIN via Keypad.
* **Proses:** ESP32 memverifikasi data.
* **Output:** Servo membuka pintu, LCD menampilkan status, dan data dikirim ke server.
* **Kelemahan:** Koneksi Wi-Fi rentan terhadap serangan beban tinggi.

### **2. Komponen Utama**

<img width="500" height="500" alt="image" src="https://github.com/user-attachments/assets/9f02431b-d100-4ea6-bf14-9ff9096d30b7" /> <img width="300" height="300" alt="image" src="https://github.com/user-attachments/assets/64c655ec-99e1-44c5-8b06-d9d8e8bb9bb0" />

* **Mikrokontroler:** NodeMCU ESP32
* **Sensor:** RFID & Keypad 4×4
* **Output:** LCD 16×2, Servo, Buzzer

---

## **Target Pengujian**

* **Perangkat Target:** ESP32
* **Layanan:** Wi-Fi 802.11
* **Vektor Serangan:** TCP/ICMP Flooding & Deauthentication

---

# **Pengujian Pertama — Serangan via SSID (WiFi DeAuth Attack)**

## **1. Pengecekan Adapter WiFi**
<img width="500" height="500" alt="image" src="https://github.com/user-attachments/assets/d39a5f52-3698-4edf-8f6a-296c7751f957" />


Memastikan adapter Wi-Fi (wlan0) terdeteksi dan siap digunakan untuk mode monitor.

## **2. Pembuatan Script DDoS (DeAuth)**
<img width="500" height="160" alt="image" src="https://github.com/user-attachments/assets/dfc57004-97f3-4ac0-b6c4-13621b98ac79" />


Script Python (wifidos.py) dibuat menggunakan editor Nano. Script berfungsi untuk melakukan DeAuth Attack.

## **3. Isi Script (Fungsi Utama)**
<img width="500" height="500" alt="image" src="https://github.com/user-attachments/assets/f89ff0a2-fd8d-44f7-9933-e0c1367d7840" />


* Mengecek apakah script dijalankan menggunakan sudo.
* Membersihkan file .csv lama dan memindahkannya ke folder backup.
* Menyiapkan adapter untuk mode monitor.
ini isi script dari gambar tersebut
[wifidos.py](https://github.com/user-attachments/files/24058844/wifidos.py)

## **4. Eksekusi Script & Deteksi Interface**
<img width="500" height="500" alt="image" src="https://github.com/user-attachments/assets/dd09c8b5-e2ac-4601-b711-6290eba1f1e7" />


Script dijalankan dengan:

```bash
sudo python3 wifidos.py
```

Adapter **wlan0** berhasil masuk mode **monitor**.

## **5. Memilih Target & Melancarkan Serangan DeAuth**
<img width="500" height="500" alt="image" src="https://github.com/user-attachments/assets/ef2375eb-d6c7-43a5-bf78-d30b157c4594" />


Program memindai jaringan sekitar dan menampilkan daftar SSID, BSSID, dan channel.

Setelah memilih target, script mengirimkan paket DeAuth secara terus-menerus:

* Memutus perangkat dari WiFi
* Menghentikan koneksi internet

## **6. Verifikasi Serangan (Ping Test)**
<img width="500" height="500" alt="image" src="https://github.com/user-attachments/assets/23c08b1c-63c4-48a0-9272-e2a89199f576" />


Perintah:

```bash
ping google.com -t
```

Hasil:

* Awalnya normal
* Saat serangan berjalan muncul **Request timed out** → koneksi putus

---

# **Pengujian Kedua — Serangan via Alamat IP (DDoS Flooding)**

## **1. Menemukan Alamat IP Smart Door Lock**
<img width="500" height="480" alt="image" src="https://github.com/user-attachments/assets/d20074ed-fe3b-4677-a69f-977419cc91a2" />


Contoh: 192.168.1.22

## **2. Menginstal Git & Cloning DDoS-Ripper**
<img width="500" height="500" alt="image" src="https://github.com/user-attachments/assets/729a59df-83a6-46b8-81cb-e21085ca4657" />


```bash
git clone https://github.com/palahsu/DDoS-Ripper.git
cd DDoS-Ripper
```

## **3. Menjalankan Serangan Packet Flooding**

<img width="500" height="500" alt="image" src="https://github.com/user-attachments/assets/279f7ff2-7744-40e3-983e-c3506d4e064c" />


```bash
python3 DRipper.py -s 192.168.1.22 -t 135
```

Parameter:

* **-s:** alamat IP target
* **-t:** turbo (kecepatan serangan)

## **4. Monitoring Serangan**
<img width="500" height="500" alt="image" src="https://github.com/user-attachments/assets/b216b110-4566-4c61-a775-a2b26b8a9df5" />


Output menunjukkan:

* Target IP: 192.168.1.22
* Port: 80 (HTTP)
* Paket terkirim terus-menerus

## **5. Verifikasi Hasil (RTO)**

Pada saat serangan berjalan:
<img width="500" height="514" alt="image" src="https://github.com/user-attachments/assets/b661e91a-1723-4e8e-a092-6fc850ff0552" />


* Balasan ping berubah dari normal menjadi **Request timed out**
* Menandakan perangkat overload dan tidak mampu merespons

---

# **Kesimpulan**

Pengujian membuktikan bahwa Smart Door Lock berbasis ESP32 **rentan** terhadap serangan DoS/DDoS:

1. Serangan DeAuth memutus perangkat dari Wi-Fi.
2. Serangan IP Flooding menyebabkan **Resource Exhaustion**.
3. Perangkat kehilangan koneksi dan gagal berfungsi selama serangan berlangsung.

Solusi mitigasi diperlukan, seperti:

* Menambahkan rate limiting
* Menggunakan WPA3 + Protected Management Frames
* Membatasi akses jaringan ke perangkat IoT

---


