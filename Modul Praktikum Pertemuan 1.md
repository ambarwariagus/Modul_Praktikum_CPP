# Modul Praktikum Pertemuan 1

# Fondasi C++, Alur Eksekusi, dan Perulangan

## Tujuan Pembelajaran

Setelah mengikuti praktikum ini, mahasiswa diharapkan mampu:

1. Menjelaskan perbedaan bahasa compiled dan interpreted.  
2. Memahami alur kompilasi program C++ hingga menjadi file executable.  
3. Menyusun dan menjalankan program C++ di VS Code menggunakan compiler (G++).  
4. Memahami anatomi tipe data numerik dan presisi tinggi.  
5. Menggunakan input/output standar C++.  
6. Menguasai logika percabangan.  
7. Mengimplementasikan perulangan sebagai dasar komputasi numerik dan  
   paralel.

# 1. Pengenalan & Setup Lingkungan

C++ adalah bahasa **compiled**. Berbeda dengan Python (interpreted), kode C++ harus melalui proses kompilasi penuh sebelum dapat dijalankan.

**Panduan Setup VS Code (Khusus Windows):**

Untuk dapat memprogram C++ di Windows, Anda perlu mengonfigurasi Visual Studio Code agar menggunakan compiler GCC C++ (g++) dan debugger GDB dari mingw-w64. Silakan ikuti langkah-langkah instalasi dan konfigurasinya pada panduan resmi berikut:

[Configure VS Code for MinGW-w64](https://code.visualstudio.com/docs/cpp/config-mingw)

## Alur Kerja:

1. **Source Code (.cpp)** → Kode yang ditulis programmer  
2. **Compiler (contoh: g++)** → Menerjemahkan ke bahasa mesin  
3. **Executable (.exe / binary)** → Program siap dijalankan

Dalam komputasi paralel, performa file binary inilah yang dioptimalkan dengan membaginya ke berbagai inti (core) prosesor.

# 2. Variabel, Tipe Data Numerik, & Input/Output

## 2.1 Konsep Variabel

Variabel adalah alokasi memori untuk menyimpan data. Dalam komputasi saintifik, pemilihan tipe data berdampak pada:

- Konsumsi memori  
- Rentang nilai  
- Presisi perhitungan

## 2.2 Tipe Data Dasar C++

| Tipe Data | Deskripsi | Ukuran Memori (Umum) | Rentang Nilai / Presisi | Contoh |
|-----------|-----------|----------------------|--------------------------|--------|
| `bool` | Nilai logika (benar/salah) | 1 byte | true (1) atau false (0) | `true` |
| `char` | Karakter tunggal | 1 byte | -128 sampai 127 (ASCII) | `'A'` |
| `int` | Bilangan bulat standar | 4 bytes | -2.147.483.648 sampai 2.147.483.647 | `100` |
| `long long` | Bilangan bulat kapasitas besar | 8 bytes | ±9 × 10¹⁸ | `9000000000000` |
| `float` | Desimal presisi tunggal | 4 bytes | ±3.4 × 10³⁸ (~7 digit presisi) | `3.14f` |
| `double` | Desimal presisi ganda | 8 bytes | ±1.7 × 10³⁰⁸ (~15–16 digit presisi) | `3.14159265358979` |

**Catatan:** Untuk komputasi saintifik, gunakan `double` karena presisinya lebih tinggi dan lebih stabil.

## 2.3 Latihan Praktik: Konversi Suhu

```cpp
#include <iostream>

int main() {  
    // Alokasi memori sebesar 8 bytes masing-masing  
    double celsius, fahrenheit;

    std::cout << "Masukkan suhu dalam Celsius: ";  
    std::cin >> celsius;

    // Menggunakan 9.0 / 5.0 agar pembagian menghasilkan desimal (double)  
    fahrenheit = (9.0 / 5.0) * celsius + 32.0;

    std::cout << "Suhu dalam Fahrenheit: " << fahrenheit << std::endl;  
    return 0;  
}
```

### Penjelasan Kode

- `#include <iostream>` → Memuat pustaka standar untuk input/output.  
- `std::cout` → Mencetak teks ke layar.  
- `std::cin` → Membaca input dari keyboard.  
- `9.0 / 5.0` → Memaksa pembagian dilakukan dalam tipe double. Jika  
  ditulis 9 / 5, hasilnya adalah 1 (pembagian integer).

### Mengapa harus menulis std::?

Penulisan `std::` adalah best practice untuk menghindari *name clashing* (bentrok nama fungsi), terutama pada proyek skala besar dan komputasi paralel.

### Instruksi Eksplorasi

1. Ubah tipe data double menjadi int. Jalankan program dan masukkan  
   nilai 35.5. Amati bagaimana C++ memotong nilai desimal.  
2. Kembalikan ke double. Ubah 9.0 / 5.0 menjadi 9 / 5. Mengapa  
   hasilnya menjadi tidak akurat?  
3. Tambahkan perintah `using namespace std;` tepat di bawah baris `#include <iostream>`. Setelah itu, hapus semua awalan `std::` pada cout, cin, dan endl. Jalankan kembali programnya. Perhatikan bagaimana kode Anda menjadi lebih ringkas, namun ingat kembali "Catatan Penting" tentang *name clashing* di tingkat komputasi lanjut.

# 3. Logika Kondisional / Percabangan

Berikut adalah operator yang sering digunakan untuk mengevaluasi kondisi pada percabangan:

| Kategori Operator | Simbol | Keterangan / Arti |
|-------------------|--------|-------------------|
| **Relasional** | `==` | Sama dengan |
|  | `!=` | Tidak sama dengan |
|  | `>`, `<` | Lebih besar dari, Lebih kecil dari |
|  | `>=`, `<=` | Lebih besar sama dengan, Lebih kecil sama dengan |
| **Logika** | `&&` | Logika **DAN** (Keduanya harus bernilai benar) |
|  | `\|\|` | Logika **ATAU** (Salah satu bernilai benar) |
|  | `!` | Logika **BUKAN** (Membalikkan nilai kondisi) |

Dalam komputasi paralel, meminimalkan percabangan dalam loop penting untuk menjaga performa.

## Latihan Praktik: Pengecekan Kategori Data

```cpp
#include <iostream>

int main() {  
    double nilai;  
    std::cout << "Masukkan nilai observasi (0 - 100): ";  
    std::cin >> nilai;

    if (nilai < 0 || nilai > 100) {  
        std::cout << "Error: Nilai di luar batas wajar (Anomali)." << std::endl;  
    } else if (nilai >= 70) {  
        std::cout << "Status: Valid dan Memenuhi Syarat." << std::endl;  
    } else {  
        std::cout << "Status: Valid tapi Tidak Memenuhi Syarat." << std::endl;  
    }

    return 0;  
}
```

### Penjelasan Kode

- `nilai < 0 || nilai > 100` → Error jika salah satu kondisi  
  terpenuhi.  
- `else if (nilai >= 70)` → Dicek jika kondisi pertama salah.  
- `else` → Dieksekusi jika semua kondisi sebelumnya salah.

### Instruksi Eksplorasi

1. Ubah operator `||` menjadi `&&`. Masukkan nilai `-5`. Mengapa tidak dianggap error?  
2. Ubah `nilai >= 70` menjadi `nilai > 70`. Masukkan `70` dan amati pergeseran status.

# 4. Perulangan (Looping)

Berikut adalah jenis-jenis perulangan di C++:

| Jenis Perulangan | Kegunaan / Karakteristik Utama |
|------------------|--------------------------------|
| `for` | Digunakan ketika jumlah iterasi (pengulangan) sudah diketahui secara pasti. |
| `while` | Digunakan ketika perulangan bergantung pada suatu kondisi (belum pasti kapan berhenti). |
| `do-while` | Mirip while, namun blok kode dijamin dieksekusi **minimal satu kali** sebelum kondisi dicek. |

Dalam komputasi paralel, struktur `for` sangat krusial karena indeksnya dapat dibagi ke beberapa core.

## Latihan Praktik 1: Menghitung Total Deret (`for` loop)

```cpp
#include <iostream>

int main() {  
    int batas_atas;  
    int total_jumlah = 0; // Akumulator

    std::cout << "Masukkan batas atas angka: ";  
    std::cin >> batas_atas;

    // for(inisialisasi; kondisi; update)  
    for (int i = 1; i <= batas_atas; i++) {  
        total_jumlah = total_jumlah + i;  
    }

    std::cout << "Total penjumlahan dari 1 hingga "  
              << batas_atas  
              << " adalah: "  
              << total_jumlah << std::endl;

    return 0;  
}
```

### Penjelasan Kode

- `total_jumlah = 0` → Akumulator harus diinisialisasi.  
- `int i = 1` → Nilai awal.  
- `i <= batas_atas` → Kondisi berhenti.  
- `i++` → Penambahan setiap iterasi. Memperbarui nilai akumulator di setiap putaran. Beri juga informasi tambahan bahwa kode ini biasanya disingkat menjadi `total_jumlah += i;` sebagai standar penulisan C++ industri.

### Instruksi Eksplorasi

1. Ubah inisialisasi menjadi `int i = 0`. Apakah hasil berubah?  
2. Ubah `i++` menjadi `i += 2`. Masukkan batas 10. Apa yang terjadi?  
3. Hapus `= 0` pada `total_jumlah`. Jalankan program dan amati hasilnya.

## Latihan Praktik 2: Validasi Input dengan `while` loop

Loop `while` sangat cocok digunakan ketika kita tidak tahu pasti berapa kali perulangan harus dilakukan, misalnya menunggu input pengguna yang benar.

```cpp
#include <iostream>

int main() {  
    int angka = -1;

    // Loop akan terus berjalan SELAMA kondisi bernilai benar (angka negatif)  
    while (angka < 0) {  
        std::cout << "Masukkan angka positif: ";  
        std::cin >> angka;

        if (angka < 0) {  
            std::cout << "Salah! Angka tidak boleh negatif.\n";  
        }  
    }

    std::cout << "Terima kasih. Anda memasukkan angka: " << angka << std::endl;  
    return 0;  
}
```

### Penjelasan Kode

- `while (angka < 0)` → Kondisi dicek **sebelum** masuk ke dalam loop. Jika nilai angka sudah positif sejak awal, maka loop tidak akan pernah dieksekusi sekalipun.

### Instruksi Eksplorasi

1. Ubah nilai inisialisasi awal menjadi `int angka = 10;`. Jalankan program. Apa yang terjadi? Mengapa program langsung selesai tanpa meminta input?

## Latihan Praktik 3: Menu Interaktif dengan `do-while` loop

Loop `do-while` menjamin bahwa blok kode di dalamnya akan dieksekusi **minimal satu kali**, terlepas dari apakah kondisinya di akhir bernilai benar atau salah.

```cpp
#include <iostream>

int main() {  
    char pilihan;

    do {  
        std::cout << "\n--- Menu Pemrosesan ---" << std::endl;  
        std::cout << "Menjalankan komputasi data..." << std::endl;  
        std::cout << "Komputasi selesai!" << std::endl;

        std::cout << "Apakah Anda ingin memproses data lagi? (Y/T): ";  
        std::cin >> pilihan;  
          
    } while (pilihan == 'Y' || pilihan == 'y');

    std::cout << "Program dihentikan." << std::endl;  
    return 0;  
}
```

### Penjelasan Kode

- `do { ... }` → C++ akan langsung mengeksekusi blok kode ini tanpa banyak tanya.  
- `while (pilihan == 'Y' || pilihan == 'y');` → Setelah dieksekusi sekali, barulah kondisi dievaluasi. Jika pengguna memasukkan huruf Y atau y, maka program melompat kembali ke atas (ke bagian do).

### Instruksi Eksplorasi

1. Coba ubah struktur `do-while` di atas menggunakan `while` loop biasa. Apa kesulitan logis yang Anda temui?

> **Petunjuk:** Anda harus memastikan variabel `pilihan` memiliki nilai awal yang benar agar bisa masuk ke dalam loop `while` untuk pertama kalinya).

# 5. Tugas Mandiri

Buat program C++ yang:

1. Meminta user memasukkan tepat 5 angka menggunakan `for` loop.  
2. Di dalam loop, gunakan `if-else` untuk mengecek apakah angka  
   positif, negatif, atau nol.  
3. Hitung jumlah total angka positif.  
4. Cetak hasil akhir setelah loop selesai.

# Ringkasan

Pada pertemuan ini mahasiswa telah mempelajari:

- Alur kompilasi C++  
- Variabel dan tipe data numerik  
- Input/output standar  
- Logika percabangan  
- Struktur perulangan sebagai fondasi komputasi numerik dan paralel  

Materi ini menjadi dasar untuk optimasi dan komputasi paralel pada pertemuan berikutnya.