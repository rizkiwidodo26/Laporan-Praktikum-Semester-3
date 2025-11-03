# <h1 align="center">Laporan Praktikum Modul 7 <br> Stack</h1>
<p align="center">Rizki Widodo - 103112400136</p>

## Dasar Teori
Stack adalah struktur data linear yang mengikuti prinsip LIFO (Last In First Out), di mana elemen terakhir yang dimasukkan akan menjadi elemen pertama yang dikeluarkan. Stack dapat diimplementasikan menggunakan dua representasi utama, yaitu representasi pointer (menggunakan linked list dengan elemen yang ditunjuk oleh pointer dan operasi terbatas pada elemen top) dan **representasi tabel (menggunakan array dengan indeks top yang menandai posisi elemen teratas). Operasi utama pada stack meliputi push untuk menambahkan elemen dan pop untuk menghapus elemen, serta primitif pendukung seperti pengecekan kekosongan stack, pembuatan stack, dan manipulasi data secara terbatas hanya melalui bagian atas stack.

## Guide

```go
#include <iostream>
using namespace std;

struct Node
{
    int data;
    Node *next;
};

bool isEmpety(Node *top)
{
    return top == nullptr;
}

void push(Node *&top, int data){
    Node *newNode = new Node;
    newNode -> data = data;
    newNode -> next = top;
    top = newNode;
}

int pop(Node *&top)
{
    if (isEmpety(top))
    {
        cout << "Stack Kosong, tidak bisa pop!" << endl;
        return 0;
    }

    int poppedData = top -> data;
    top = top -> next;

    Node *temp;
    return poppedData;
}

void show (Node *top)
{
    if (isEmpety(top))
    {
        cout << "Stack kosong," << endl;
        return;
    }
    cout << "TOP -> ";
    Node *temp = top;

    while (temp != nullptr)
    {
        cout << temp->data << "-> ";
        temp = temp->next;
    }

    cout << "NULL" << endl;

}

int main()
{
    Node *stack = nullptr;

    push(stack, 10);
    push(stack, 20);
    push(stack, 30);

    cout << "Menampilkan isi stack:" << endl;
    show (stack);

    cout << "pop;" << pop(stack) << endl;

    cout << "Menampilkan sisa Stack:" << endl;
    show(stack);

    return 0;
}
```


## Unguide

### Soal 1

### stack.h
```go
#ifndef STACK_H
#define STACK_H

typedef int infotype;

struct Stack {
    infotype info[20];
    int top;
};

void createStack(Stack &S);
void push(Stack &S, infotype x);
infotype pop(Stack &S);
void printInfo(Stack S);
void balikStack(Stack &S);

#endif
```

### stack.cpp
```go
#include <iostream>
#include "stack.h"
using namespace std;

void createStack(Stack &S) {
    S.top = -1;  // Top = -1 menandakan stack kosong
}

void push(Stack &S, infotype x) {
    if (S.top < 19) {  // Cek apakah stack belum penuh
        S.top++;
        S.info[S.top] = x;
    } else {
        cout << "Stack penuh!" << endl;
    }
}

infotype pop(Stack &S) {
    if (S.top >= 0) {  // Cek apakah stack tidak kosong
        infotype x = S.info[S.top];
        S.top--;
        return x;
    } else {
        cout << "Stack kosong!" << endl;
        return -1;
    }
}

void printInfo(Stack S) {
    cout << "[TOP] ";
    for (int i = S.top; i >= 0; i--) {
        cout << S.info[i] << " ";
    }
    cout << endl;
}

void balikStack(Stack &S) {
    Stack temp;
    createStack(temp);
    
    // Pindahkan semua elemen dari S ke temp
    while (S.top >= 0) {
        push(temp, pop(S));
    }
    
    // Balik urutan dengan stack kedua
    Stack temp2;
    createStack(temp2);
    while (temp.top >= 0) {
        push(temp2, pop(temp));
    }
    
    // Kembalikan ke stack asli (sudah terbalik)
    while (temp2.top >= 0) {
        push(S, pop(temp2));
    }
}
```

### main.cpp
```go
#include <iostream>
#include "stack.h"
using namespace std;

int main() {
    cout << "Hello world!" << endl;
    Stack S;
    createStack(S);
    push(S, 3);
    push(S, 4);
    push(S, 8);
    pop(S);
    push(S, 2);
    push(S, 3);
    pop(S);
    push(S, 9);
    printInfo(S);
    cout << "balik stack" << endl;
    balikStack(S);
    printInfo(S);
    return 0;
}
```

> Output
> ![Screenshot bagian x](Output/Output_no1.png)

PENJELASAN SINGKAT PROGRAM NO 1

Program stack nomor 1 telah berhasil diimplementasikan menggunakan array dengan prinsip LIFO (Last In First Out). Stack ini memiliki kapasitas 20 elemen dan variabel 'top' sebagai penanda posisi teratas. Program melakukan serangkaian operasi push dan pop: dimulai dari push 3, 4, 8 membentuk stack [8,4,3], kemudian pop mengambil 8 menjadi [4,3], push 2 dan 3 menjadi [3,2,4,3], pop mengambil 3 menjadi [2,4,3], dan terakhir push 9 menghasilkan stack akhir [9,2,4,3]. Fungsi balikStack berhasil membalik urutan menjadi [3,4,2,9] menggunakan dua stack sementara. Output program sesuai ekspektasi, membuktikan semua fungsi dasar stack (createStack, push, pop, printInfo, balikStack) bekerja benar dengan prinsip elemen terakhir masuk pertama keluar.

### Soal 2

### stack.h
```go
#ifndef STACK_H
#define STACK_H

typedef int infotype;

struct Stack {
    infotype info[20];
    int top;
};

void createStack(Stack &S);
void push(Stack &S, infotype x);
infotype pop(Stack &S);
void printInfo(Stack S);
void balikStack(Stack &S);
void pushAscending(Stack &S, infotype x);  // PASTIKAN BARIS INI ADA

#endif
```

### stack.cpp
```go
#include <iostream>
#include "stack.h"
using namespace std;

void createStack(Stack &S) {
    S.top = -1;
}

void push(Stack &S, infotype x) {
    if (S.top < 19) {
        S.top++;
        S.info[S.top] = x;
    } else {
        cout << "Stack penuh!" << endl;
    }
}

infotype pop(Stack &S) {
    if (S.top >= 0) {
        infotype x = S.info[S.top];
        S.top--;
        return x;
    } else {
        cout << "Stack kosong!" << endl;
        return -1;
    }
}

void printInfo(Stack S) {
    cout << "[TOP] ";
    for (int i = S.top; i >= 0; i--) {
        cout << S.info[i] << " ";
    }
    cout << endl;
}

void balikStack(Stack &S) {
    Stack temp;
    createStack(temp);
    
    while (S.top >= 0) {
        push(temp, pop(S));
    }
    
    Stack temp2;
    createStack(temp2);
    while (temp.top >= 0) {
        push(temp2, pop(temp));
    }
    
    while (temp2.top >= 0) {
        push(S, pop(temp2));
    }
}

// FUNGSI PUSH ASCENDING UNTUK NOMOR 2
void pushAscending(Stack &S, infotype x) {
    if (S.top == -1 || x >= S.info[S.top]) {
        push(S, x);
    } else {
        infotype temp = pop(S);
        pushAscending(S, x);
        push(S, temp);
    }
}
```

### main.cpp
```go
#include <iostream>
#include "stack.h"
using namespace std;

int main() {
    cout << "Hello world!" << endl;
    Stack S;
    createStack(S);
    pushAscending(S, 3);
    pushAscending(S, 4);
    pushAscending(S, 8);
    pushAscending(S, 2);
    pushAscending(S, 3);
    pushAscending(S, 9);
    printInfo(S);
    cout << "balik stack" << endl;
    balikStack(S);
    printInfo(S);
    return 0;
}
```

> Output
> ![Screenshot bagian x](Output/Output_no2.png)

PENJELASAN SINGKAT PROGRAM NOMOR 2

Program nomor 2 menambahkan fungsi pushAscending yang memodifikasi operasi push biasa menjadi penyisipan terurut, dimana setiap elemen baru dimasukkan ke dalam stack dengan menjaga urutan descending dari top ke bottom. Fungsi ini bekerja secara rekursif dengan membandingkan elemen input dengan elemen top - jika lebih kecil maka elemen top disimpan sementara, pencarian posisi dilanjutkan secara rekursif sampai ditemukan posisi yang tepat, kemudian elemen yang disimpan dikembalikan, sehingga ketika stack dibalik akan menghasilkan urutan ascending yang sempurna.

### Soal 3


### stack.h
```go
#ifndef STACK_H
#define STACK_H

typedef int infotype;

struct Stack {
    infotype info[20];
    int top;
};

void createStack(Stack &S);
void push(Stack &S, infotype x);
infotype pop(Stack &S);
void printInfo(Stack S);
void balikStack(Stack &S);
void getInputStream(Stack &S);  // PASTIKAN ADA BARIS INI

#endif
```

### stack.cpp
```go
#include <iostream>
#include "stack.h"
using namespace std;

void createStack(Stack &S) {
    S.top = -1;
}

void push(Stack &S, infotype x) {
    if (S.top < 19) {
        S.top++;
        S.info[S.top] = x;
    } else {
        cout << "Stack penuh!" << endl;
    }
}

infotype pop(Stack &S) {
    if (S.top >= 0) {
        infotype x = S.info[S.top];
        S.top--;
        return x;
    } else {
        cout << "Stack kosong!" << endl;
        return -1;
    }
}

void printInfo(Stack S) {
    cout << "[TOP] ";
    for (int i = S.top; i >= 0; i--) {
        cout << S.info[i] << " ";
    }
    cout << endl;
}

void balikStack(Stack &S) {
    Stack temp;
    createStack(temp);
    
    while (S.top >= 0) {
        push(temp, pop(S));
    }
    
    Stack temp2;
    createStack(temp2);
    while (temp.top >= 0) {
        push(temp2, pop(temp));
    }
    
    while (temp2.top >= 0) {
        push(S, pop(temp2));
    }
}

// FUNGSI NOMOR 3 - PASTIKAN ADA
void getInputStream(Stack &S) {
    char input;
    cout << "Masukkan angka (tekan Enter untuk selesai): ";
    
    while (cin.get(input)) {
        if (input == '\n') break;
        
        if (input >= '0' && input <= '9') {
            int angka = input - '0';
            push(S, angka);
        }
    }
}
```

### main.cpp
```go
#include <iostream>
#include "stack.h"
using namespace std;

int main() {
    cout << "Hello world!" << endl;
    Stack S;
    createStack(S);
    getInputStream(S);  // HANYA INI
    printInfo(S);
    cout << "balik stack" << endl;
    balikStack(S);
    printInfo(S);
    return 0;
}
```

> Output
> ![Screenshot bagian x](Output/Output_no3.png)

PENJELASAN SINGKAT PROGRAM NOMOR 

Program nomor 3 menggunakan fungsi getInputStream yang membaca input user secara real-time karakter per karakter menggunakan cin.get(). Setiap digit angka yang dimasukkan user secara otomatis dikonversi dari char ke integer dan langsung dipush ke dalam stack. Proses pembacaan input akan berhenti ketika user menekan tombol Enter (\n), kemudian program menampilkan stack yang terbentuk dan membalik urutannya. Contoh input "4729601" menghasilkan stack "[TOP] 1 0 6 9 2 7 4" karena prinsip LIFO dimana digit terakhir yang dimasukkan (1) berada di posisi teratas, dan setelah dibalik menjadi "[TOP] 4 7 2 9 6 0 1" yang merepresentasikan urutan input asli.

## Referensi
1. https://www.w3schools.com/cpp/cpp_functions.asp
2. https://www.w3schools.com/cpp/cpp_arrays.asp
3. https://www.w3schools.com/cpp/cpp_user_input.asp
4. https://www.w3schools.com/cpp/cpp_while_loop.asp
5. https://www.w3schools.com/cpp/cpp_stacks.asp
