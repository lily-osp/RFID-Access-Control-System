# Sistem Kontrol Akses RFID dengan Keluaran Tambahan

Kode ini menggunakan papan Arduino, pembaca RFID (MFRC522), dan beberapa komponen (LED, relay, dan sakelar input) untuk mengontrol akses ke area atau perangkat tertentu. Kode ini membaca ID unik dari tag RFID dan membandingkannya dengan tag yang telah diotorisasi sebelumnya. Jika ID cocok, akses diberikan dan LED hijau menyala, relay aktif, dan LED biru menyala. Jika ID tidak cocok, akses ditolak dan LED hijau tetap mati, relay tetap mati, dan LED biru tetap mati. Selain itu, kode ini juga membaca status sakelar input. Jika sakelar dalam posisi tinggi, akses diberikan dan LED serta relay akan menyala. Jika sakelar dalam posisi rendah, akses ditolak dan LED serta relay akan tetap mati. Kontrol akses dapat dipicu dengan mengirim perintah melalui serial monitor.

## Perangkat Keras

- Papan Arduino (misalnya Uno, Mega, Nano)
- Pembaca RFID MFRC522
- Tag RFID
- LED hijau
- LED merah
- LED biru
- Relay
- Sakelar input
- Kabel jumper

## Koneksi Pin

|MFRC522|Arduino|
|-------|-------|
|SDA    |10     |
|SCK    |13     |
|MOSI   |11     |
|MISO   |12     |
|IRQ    |tidak terhubung|
|GND    |GND    |
|RST    |9      |
|3.3V   |3.3V   |

|LED    |Arduino|
|-------|-------|
|Hijau  |2      |
|Merah  |3      |
|Relay  |4      |
|Sakelar input  |5      |
|Biru   |6      |

## Persiapan Kode

1. Pasang pustaka MFRC522 di Arduino IDE (Sketch > Include Library > MFRC522).
2. Sambungkan pembaca RFID, LED, relay, dan sakelar input ke papan Arduino sesuai dengan koneksi pin yang dijelaskan di atas.
3. Unggah kode ke papan Arduino.
4. Buka serial monitor (Tools > Serial Monitor) dan atur ke "Newline" dan "9600 baud".
5. Tempatkan tag RFID di atas pembaca, ID unik tag tersebut akan ditampilkan di serial monitor.
6. Di dalam kode, perbarui baris content.substring(1) == "99 46 87 8D" dengan UID tag RFID yang ingin diberi akses.
7. Simpan perubahan dan unggah ulang kode ke papan Arduino.
8. Kirim perintah "1" ke serial monitor untuk memicu kontrol akses.
9. Kirim perintah "2" ke serial monitor untuk menolak akses.
10. Kirim perintah "3" ke serial monitor untuk mematikan semua keluaran.

## Pengujian Kode

1. Letakkan tag RFID yang telah diotorisasi di atas pembaca, sakelar input berada dalam posisi tinggi, lalu kirim perintah "1" ke serial monitor. LED hijau harus menyala, LED merah harus mati, relay harus aktif, dan

 LED biru harus menyala selama 3,5 detik.
2. Letakkan tag RFID yang telah diotorisasi di atas pembaca, sakelar input berada dalam posisi rendah, lalu kirim perintah "1" ke serial monitor. LED hijau harus menyala, LED merah harus menyala, relay harus mati, dan LED biru harus mati selama 0,5 detik.
3. Letakkan tag RFID yang tidak diotorisasi di atas pembaca, sakelar input berada dalam posisi tinggi, lalu kirim perintah "1" ke serial monitor. LED hijau harus menyala, LED merah harus menyala, relay harus mati, dan LED biru harus mati selama 0,5 detik.
4. Letakkan tag RFID yang tidak diotorisasi di atas pembaca, sakelar input berada dalam posisi rendah, lalu kirim perintah "1" ke serial monitor. LED hijau harus mati, LED merah harus menyala, relay harus mati, dan LED biru harus mati selama 0,5 detik.
5. Kirim perintah "2" ke serial monitor. LED hijau harus mati, LED merah harus menyala, relay harus mati, dan LED biru harus mati selama 0,5 detik.
6. Kirim perintah "3" ke serial monitor. Semua keluaran harus mati.

## Catatan

- Anda dapat menambahkan lebih banyak tag RFID dengan menambahkan pernyataan `else if` dalam kode dan membandingkan UID mereka dengan tag yang dibaca.
- Waktu jeda untuk akses yang diizinkan dan ditolak dapat disesuaikan sesuai dengan durasi yang diinginkan.
- Sakelar input dapat digantikan dengan perangkat masukan lain yang dapat dihubungkan ke Arduino seperti tombol, pembaca RFID, atau sensor PIR.

## Penggunaan Tambahan

Proyek ini hanya salah satu contoh dari banyaknya penerapan [google theachable machine](https://teachablemachine.withgoogle.com/). Untuk menggunakan proyek ini, Anda dapat:

1. Mem-flash arduino 1 dengan program pertama.
2. Mem-flash arduino 2 dengan program kedua.
3. Menghubungkan arduino 2 ke PC/laptop.
4. Membuka [program p5 serial control](https://github.com/p5-serial/p5.serialcontrol/releases).
5. Menghubungkan ke pin serial arduino.
6. Membuka [p5.js web editor](https://editor.p5js.org/1999AZZAR/sketches/Te50F4Q3-).
7. Mengedit port serial dalam p5.js web editor agar sama dengan dalam p5 serial control.
8. Menjalankan p5.js web editor.

## Diagram Alir

```mermaid
graph TD
subgraph Perangkat 1
A1[Mulai Perangkat 1] --> B1[Persiapan Perangkat 1]
B1 --> C1{Kartu Baru?}
C1 -- Tidak --> B1
C1 -- Ya --> D1{Baca Kartu}
D1 -- Sukses --> E1[Cetak UID]
E1 --> F1[Periksa Akses]
F1 -- Diizinkan --> G1[Akses Diberikan]
G1 --> H1{Lanjutkan?}
H1 -- Ya

 --> C1
H1 -- Tidak --> I1[Akhir Perangkat 1]
F1 -- Ditolak --> J1[Akses Ditolak]
J1 --> H1
G1 --> K1[Tetapkan nilai incontrol]
J1 --> K1
end
subgraph Perangkat 2
A2[Mulai Perangkat 2] --> B2[Persiapan Perangkat 2]
B2 --> C2{Serial Tersedia?}
C2 -- Tidak --> G2[Penundaan]
C2 -- Ya --> D2[Baca Serial]
D2 --> E2{Hasil}
E2 -- '1' --> F2{incontrol == HIGH?}
F2 -- Ya --> K2[Akses Diberikan]
K2 --> G2[Penundaan]
F2 -- Tidak --> L[Akses Ditolak]
L --> G2[Penundaan]
E2 -- '2' --> H2{incontrol == HIGH?}
H2 -- Ya --> I2[Akses Ditolak]
I2 --> G2[Penundaan]
H2 -- Tidak --> J2[Akses Ditolak]
J2 --> G2[Penundaan]
E2 -- '3' --> M{incontrol == HIGH?}
M -- Ya --> N[Akses Ditolak]
N --> G2[Penundaan]
M -- Tidak --> O[Akses Ditolak]
O --> G2[Penundaan]
G2 --> C2
G2 --> P[Akhir Perangkat 2]

K1 --> F2
K1 --> H2
K1 --> M
end
```

## Galeri

<!DOCTYPE html>
<html>
<!-- Image Grid Gallery -->
<div class="grid-gallery" style="display: grid; grid-template-columns: repeat(3, 1fr); grid-gap: 5px; justify-items: center;">

<!-- Image 1 -->
<div>
  <img src="https://github.com/1999AZZAR/RFID-Access-Control-System/raw/master/pic/Slide1.jpg" alt="Image 1" style="height: 300px;">
</div>

<!-- Image 2 -->
<div>
  <img src="https://github.com/1999AZZAR/RFID-Access-Control-System/raw/master/pic/Slide2.jpg" alt="Image 2" style="height: 300px;">
</div>

<!-- Image 3 -->
<div>
  <img src="https://github.com/1999AZZAR/RFID-Access-Control-System/raw/master/pic/Slide3.jpg" alt="Image 3" style="height: 300px;">
</div>

<!-- Image 4 -->
<div>
  <img src="https://github.com/1999AZZAR/RFID-Access-Control-System/raw/master/pic/Slide4.jpg" alt="Image 4" style="height: 300px;">
</div>

<!-- Image 5 -->
<div>
  <img src="https://github.com/1999AZZAR/RFID-Access-Control-System/raw/master/pic/Slide5.jpg" alt="Image 5" style="height: 300px;">
</div>

<!-- Image 6 -->
<div>
  <img src="https://github.com/1999AZZAR/RFID-Access-Control-System/raw/master/pic/Slide6.jpg" alt="Image 6" style="height: 300px;">
</div>

<!-- Image 7 -->
<div>
  <img src="https://github.com/1999AZZAR/RFID-Access-Control-System/raw/master/pic/Slide7.jpg" alt="Image 7" style="height: 300px;">
</div>

<!-- Image 8 -->
<div>
  <img src="https://github.com/1999AZZAR/RFID-Access-Control-System/raw/master/pic/Slide8.jpg" alt="Image 8" style="height: 300px;">
</div>

<!-- Image 9 -->
<div>
  <img src="https://github.com/1999AZZAR/RFID-Access-Control-System/raw/master/pic/Slide9.jpg" alt="Image 9" style="height: 300px;">
</div>

<!-- Image 10 -->
<div>
  <img src="https://github.com/1999AZZAR/RFID-Access-Control-System/raw/master/pic/Slide10.jpeg" alt="Image 10" style="height: 300px;">
</div>

<!-- Image 11 -->
<div>
  <img src="https://github.com/1999AZZAR/RFID-Access-Control-System/raw/master/pic/Slide11.jpeg" alt="Image 11" style="height: 300px;">
</div>

<!-- Image 12 -->
<div>
  <img src="https://github.com/1999AZZAR/RFID-Access-Control-System/raw/master/pic/Slide12.jpeg" alt="Image 12" style="height: 300px;">
</div>

</div>
</html>
