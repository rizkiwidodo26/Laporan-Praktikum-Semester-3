# <h1 align="center">Laporan Praktikum Modul 2 <br> Nama Modul</h1>
<p align="center">Rizki Widodo - 103112400136</p>

## Dasar Teori
Berdasarkan modul praktikum, tipe data dan variabel merupakan konsep fundamental dalam pemrograman Go. Tipe data mendefinisikan jenis nilai yang dapat disimpan, seperti int untuk bilangan bulat, float64 untuk bilangan desimal, dan string untuk teks. Variabel berfungsi sebagai wadah penyimpanan yang dideklarasikan dengan keyword var atau short declaration :=. Program menggunakan fmt.Scan() untuk input data dan fmt.Printf() untuk output, sementara operasi aritmatika seperti penjumlahan dan perkalian digunakan untuk perhitungan matematis termasuk luas lingkaran dengan math.Pi. Manipulasi string memungkinkan penggabungan teks, dan teknik swap variabel dengan bantuan variabel sementara digunakan untuk pertukaran nilai. Pemahaman ini menjadi pondasi essential sebelum melanjutkan ke struktur data yang lebih kompleks.

## Unguided

### soal 1
1. Buatlah sebuah program untuk melakukan transpose pada sebuah matriks persegi berukuran 3x3. Operasi transpose adalah mengubah baris menjadi kolom dan sebaliknya. Inisialisasi matriks awal di dalam kode, kemudian buat logika untuk melakukan transpose dan simpan hasilnya ke dalam matriks baru. Terakhir, tampilkan matriks awal dan matriks hasil transpose.

Contoh Output:

Matriks Awal:
1 2 3
4 5 6
7 8 9

Matriks Hasil Transpose:
1 4 7
2 5 8
3 6 9

### Soal 1

```go
#include <iostream>
using namespace std;

int main() {
    // Inisialisasi matriks awal 3x3
    int matriks[3][3] = {{1, 2, 3}, {4, 5, 6}, {7, 8, 9}};
    int transpose[3][3];
    
    // Menampilkan matriks awal
    cout << "Matriks Awal:" << endl;
    for (int i = 0; i < 3; i++) {
        for (int j = 0; j < 3; j++) {
            cout << matriks[i][j] << " ";
        }
        cout << endl;
    }
    
    // Melakukan operasi transpose
    for (int i = 0; i < 3; i++) {
        for (int j = 0; j < 3; j++) {
            transpose[j][i] = matriks[i][j];
        }
    }
    
    // Menampilkan matriks hasil transpose
    cout << "\nMatriks Hasil Transpose:" << endl;
    for (int i = 0; i < 3; i++) {
        for (int j = 0; j < 3; j++) {
            cout << transpose[i][j] << " ";
        }
        cout << endl;
    }
    
    return 0;
}
```

> Output
> ![Screenshot bagian x](output/no1.png (2))
> %% Untuk mencantumkan screenshot, tidak boleh ada spasi di urlnya `()`, penamaan file bebas asal gak sara dan mudah dipahami aja,, dan jangan lupa hapus komen ini yah%%

Penjelasan ttg kode kalian disini

### Soal 2

soal nomor 2A

```go
package main

func main() {
	fmt.Println("kode untuk soal nomor 2A")
}
```

> Output
> ![Screenshot bagian x](output/screenshot_soal2A.png)

penjelasan kode

Kalau adalanjutan di lanjut disini aja

soal nomor 2B

```go
package main

func main() {
	fmt.Println("kode untuk soal nomor 2B")
}
```

> Output
> ![Screenshot bagian x](output/screenshot_soal2B.png)

penjelasan bedanya sesuai soal

## Referensi

1. https://en.wikipedia.org/wiki/Data_structure (diakses blablabla)
