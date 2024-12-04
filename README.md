# Contoh Kontensi Sumber Daya dengan FreeRTOS

Program ini adalah implementasi multitasking menggunakan FreeRTOS pada mikrokontroler STM32. Program ini menunjukkan cara dua task (GreenFlashTask dan RedFlashTask) mengakses sumber daya bersama secara bergantian, dengan menangani kontensi sumber daya menggunakan indikator LED.

---

## Fitur

1. **GreenFlashTask**: 
   - Mengontrol LED hijau dan mengakses sumber daya bersama.
   - Memiliki jeda 500 ms antar operasi.
2. **RedFlashTask**:
   - Mengontrol LED merah dengan prioritas lebih rendah dari GreenFlashTask.
   - Memiliki jeda 100 ms antar operasi.
3. **Kontensi Sumber Daya**:
   - Jika dua task mencoba mengakses sumber daya secara bersamaan, LED biru akan menyala sebagai indikator konflik.
4. **Manajemen Prioritas**:
   - GreenFlashTask memiliki prioritas lebih tinggi dibandingkan RedFlashTask.

---

## Cara Kerja Program

### 1. **Pengaturan Prioritas**
- **GreenFlashTask** memiliki prioritas normal (`osPriorityNormal`).
- **RedFlashTask** memiliki prioritas di bawah normal (`osPriorityBelowNormal`).

### 2. **Logika Task**
#### a. **GreenFlashTask**
- **Fungsi**: Mengontrol LED hijau dan mencoba mengakses sumber daya bersama.
- **Alur Kerja**:
  1. Menyalakan LED hijau.
  2. Mengakses sumber daya melalui fungsi `AccessSharedData()`.
  3. Mematikan LED hijau.
  4. Menunggu selama 500 ms sebelum melanjutkan.

#### b. **RedFlashTask**
- **Fungsi**: Mengontrol LED merah dan mencoba mengakses sumber daya bersama.
- **Alur Kerja**:
  1. Menyalakan LED merah.
  2. Mengakses sumber daya melalui fungsi `AccessSharedData()`.
  3. Mematikan LED merah.
  4. Menunggu selama 100 ms sebelum melanjutkan.

#### c. **Default Task**
- Tidak melakukan fungsi spesifik, hanya bertindak sebagai placeholder dengan prioritas terendah.

### 3. **Akses Sumber Daya Bersama**
- **Flag Sumber Daya**:
  - Variabel `StartFlag` digunakan untuk menunjukkan status sumber daya:
    - `1`: Sumber daya tersedia.
    - `0`: Sumber daya sedang digunakan.
- **Fungsi `AccessSharedData()`**:
  - Jika `StartFlag == 1`, task dapat mengakses sumber daya dan mengubah status menjadi `0`.
  - Jika `StartFlag == 0`, terjadi kontensi dan LED biru menyala untuk menunjukkan konflik.

### 4. **Simulasi Operasi Sumber Daya**
- Operasi sumber daya disimulasikan menggunakan delay dengan fungsi `SimulateReadWriteOperation()`.

---

## Cara Menjalankan

### 1. **Konfigurasi Hardware**
- Sambungkan tiga LED ke pin GPIO mikrokontroler:
  - LED Hijau: `GPIO_PIN_GREEN`.
  - LED Merah: `GPIO_PIN_RED`.
  - LED Biru: `GPIO_PIN_BLUE` (indikator kontensi).

### 2. **Bangun dan Flash Program**
- Import proyek ini ke STM32CubeIDE.
- Kompilasi dan unggah ke mikrokontroler STM32 Anda.

### 3. **Pengamatan**
- LED hijau dan merah akan menyala bergantian berdasarkan prioritas.
- Jika terjadi kontensi saat akses sumber daya, LED biru akan menyala untuk beberapa waktu.

---


https://github.com/user-attachments/assets/1b80f868-5d48-4852-bfc3-5b48d3de91a1

