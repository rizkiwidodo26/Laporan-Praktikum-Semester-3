# <h1 align="center">Laporan Praktikum Modul 6 <br> Doubly Linked List (Bagian Pertama)</h1>
<p align="center">Rizki Widodo - 103112400136</p>

## Dasar Teori

Doubly Linked List merupakan struktur data linear yang terdiri dari serangkaian elemen yang saling terhubung melalui pointer, dimana setiap elemen (node) memiliki dua penunjuk yaitu next yang menunjuk ke elemen berikutnya dan prev yang menunjuk ke elemen sebelumnya, serta dua pointer utama first dan last yang masing-masing menunjuk ke elemen pertama dan terakhir dalam list. Struktur ini memungkinkan traversing (penelusuran) dua arah (maju dan mundur) serta operasi penyisipan (insert) dan penghapusan (delete) yang lebih fleksibel dibanding Single Linked List, baik di posisi pertama (insertFirst/deleteFirst), terakhir (insertLast/deleteLast), maupun setelah atau sebelum elemen tertentu (insertAfter/insertBefore dan deleteAfter/deleteBefore), dengan kompleksitas waktu O(1) untuk operasi di ujung list jika memiliki akses langsung ke node yang dituju.

## Guide

```go
#include <iostream>
using namespace std;

struct Node {
    int data;
    Node* prev;
    Node* next;
};

Node* head = nullptr;
Node* tail = nullptr;

void insertDepan(int data) {
    Node* newNode = new Node();
    newNode->data = data;
    newNode->prev = nullptr;
    newNode->next = head;

    if (head != nullptr)
       head->prev = newNode;
    else
       tail = newNode;

    head = newNode;
    cout << "Data " << data << " berhasil ditambahkan di depan. \n";
}

void insertBelakang(int data) {
    Node* newNode = new Node();
    newNode->data = data;
    newNode->next = nullptr;
    newNode->prev = tail;

    if (tail != nullptr)
        tail->next = newNode;
    else
        head = newNode;

    tail = newNode;
    cout << "Data " << data << " berhasil ditambahkan di belakang.\n";
}

void insertSetelah(int target, int data) {
    Node* current = head;
    while (current != nullptr && current ->data != target)
        current = current->next;

    if(current == nullptr) {
        cout << "Data " << target << " tidak ditemukan.\n";
        return;
    }

    Node* newNode = new Node();
    newNode->data = data;
    newNode->next = current->next;
    newNode->prev = current;

    if (current->next != nullptr)
        current->next->prev = newNode;
    else
        tail = newNode;

    current->next = newNode;
    cout << "Data " << data << " berhasil disisipkan setelah " << target << ".\n";
}

void hapusDepan() {
    if (head == nullptr) {
        cout << "List kosong.\n";
        return;
    }

    Node* temp = head;
    head = head->next;

    if (head != nullptr)
        head->prev = nullptr;
    else
        tail = nullptr;

    cout << "Data " << temp->data << " dihapus dari depan.\n";
    delete temp;
}

void hapusBelakang() {
    if  (tail == nullptr) {
        cout << "List kosong.\n";
        return;
    }

    Node* temp = tail;
    tail = tail->prev;

    if (tail != nullptr)
        tail->next = nullptr;
    else
        head = nullptr;
    
    cout << "Data " << temp->data << " dihapus dari belakang.\n";
    delete temp;
}

void hapusData(int target) {
    Node* current = head;
    while (current != nullptr && current->data != target)
        current = current->next;

    if (current == nullptr) {
        cout << "Data " << target << " tidak ditemukan.\n";
        return;
    }

    if (current == head)
        hapusDepan();
    else if (current == tail)
        hapusBelakang();
    else {
        current->prev->next = current->next;
        current->next->prev = current->prev;
        cout << "Data " << target << " dihapus.\n";
        delete current;
    }
}
void updateData(int oldData, int newData) {
    Node* current = head;
    while (current != nullptr && current->data != oldData)
        current = current->next;

    if (current == nullptr) {
        cout << "Data " << oldData << " tidak ditemukan.\n";
        return;
    }

    current->data = newData;
    cout << "Data " << oldData << " diubah menjadi " << newData << ".\n";
}
void tampilDepan() {
    if (head == nullptr) {
        cout << "List kosong.\n";
        return;
    }

    cout << "Isi list (dari depan): ";
    Node* current = head;
    while (current != nullptr) {
        cout << current->data << " ";
        current = current->next;
    }
    cout << "\n";
}

// ====================================
// Fungsi: Tampilkan dari belakang
// ====================================
void tampilBelakang() {
    if (tail == nullptr) {
        cout << "List kosong.\n";
        return;
    }

    cout << "Isi list (dari belakang): ";
    Node* current = tail;
    while (current != nullptr) {
        cout << current->data << " ";
        current = current->prev;
    }
    cout << "\n";
}

// ====================================
// MAIN PROGRAM (MENU INTERAKTIF)
// ====================================
int main() {
    int pilihan, data, target, oldData, newData;

    do {
        cout << "\n===== MENU DOUBLE LINKED LIST =====\n";
        cout << "1. Insert Depan\n";
        cout << "2. Insert Belakang\n";
        cout << "3. Insert Setelah Data\n";
        cout << "4. Hapus Depan\n";
        cout << "5. Hapus Belakang\n";
        cout << "6. Hapus Data Tertentu\n";
        cout << "7. Update Data\n";
        cout << "8. Tampil dari Depan\n";
        cout << "9. Tampil dari Belakang\n";
        cout << "0. Keluar\n";
        cout << "===================================\n";
        cout << "Pilih menu: ";
        cin >> pilihan;

        switch (pilihan) {
            case 1:
                cout << "Masukkan data: ";
                cin >> data;
                insertDepan(data);
                break;
            case 2:
                cout << "Masukkan data: ";
                cin >> data;
                insertBelakang(data);
                break;
            case 3:
                cout << "Masukkan data target: ";
                cin >> target;
                cout << "Masukkan data baru: ";
                cin >> data;
                insertSetelah(target, data);
                break;
            case 4:
                hapusDepan();
                break;
            case 5:
                hapusBelakang();
                break;
            case 6:
                cout << "Masukkan data yang ingin dihapus: ";
                cin >> target;
                hapusData(target);
                break;
            case 7:
                cout << "Masukkan data lama: ";
                cin >> oldData;
                cout << "Masukkan data baru: ";
                cin >> newData;
                updateData(oldData, newData);
                break;
            case 8:
                tampilDepan();
                break;
            case 9:
                tampilBelakang();
                break;
            case 0:
                cout << "👋 Keluar dari program.\n";
                break;
            default:
                cout << "Pilihan tidak valid.\n";
        }

    } while (pilihan != 0);

    return 0;
}
```


## Unguide

### Soal 1
Buatlah ADT Doubly Linked list sebagai berikut di dalam file “Doublylist.h”:

```go
Type infotype : kendaraan <
    nopol : string
    warna : string
    thnBuat : integer
>
Type address : pointer to ElmList
Type ElmList <
    info : infotype
    next : address
    prev : address
>

Type List <
    First : address
    Last : address
>

procedure CreateList( input/output L : List )
function alokasi( x : infotype ) → address
procedure dealokasi(input/output P : address )
procedure printInfo( input L : List )
procedure insertLast(input/output L : List,  
   input P : address )
```
Buatlah implementasi ADT Doubly Linked list pada file “Doublylist.cpp” dan coba hasil implementasi ADT pada file “main.cpp”.

> Contoh Output:
``` Output
masukkan nomor polisi: D001
masukkan warna kendaraan: hitam
masukkan tahun kendaraan: 90
masukkan nomor polisi: D003
masukkan warna kendaraan: putih
masukkan tahun kendaraan: 70
masukkan nomor polisi: D001
masukkan warna kendaraan: merah
masukkan tahun kendaraan: 80
nomor polisi sudah terdaftar
masukkan nomor polisi: D004
masukkan warna kendaraan: kuning
masukkan tahun kendaraan: 90
DATA LIST 1
no polisi : D004
warna     : kuning
tahun     : 90
no polisi : D003
warna     : putih
tahun     : 70
no polisi : D001
warna     : hitam
tahun     : 90
```

## doublylist.h
```go
#ifndef DOUBLYLIST_H
#define DOUBLYLIST_H

#include <string>
using namespace std;

struct kendaraan {
    string nopol;
    string warna;
    int thnBuat;
};

typedef kendaraan infotype;
typedef struct Elmlist *address;

struct Elmlist {
    infotype info;
    address next;
    address prev;
};

struct List {
    address first;
    address last;
};

void createList(List &L);
address alokasi(infotype x);
void dealokasi(address &P);
void printInfo(List L);
void insertLast(List &L, address P);
address findElm(List L, string nopol);
void deleteFirst(List &L, address &P);
void deleteLast(List &L, address &P);
void deleteAfter(address Prec, address &P);

#endif
```

## doublylist.cpp

```go
#include "Doublylist.h"
#include <iostream>
using namespace std;

void createList(List &L) {
    L.first = NULL;
    L.last = NULL;
}

address alokasi(infotype x) {
    address P = new Elmlist;
    P->info = x;
    P->next = NULL;
    P->prev = NULL;
    return P;
}

void dealokasi(address &P) {
    delete P;
}

void printInfo(List L) {
    cout << "\nDATA LIST 1\n" << endl;
    
    if (L.first == NULL) {
        cout << "List kosong" << endl;
        return;
    }
    
    address temp = L.first;
    while (temp != NULL) {
        cout << "no polisi : " << temp->info.nopol << endl;
        cout << "warna : " << temp->info.warna << endl;
        cout << "tahun : " << temp->info.thnBuat << endl;
        temp = temp->next;
    }
}

void insertLast(List &L, address P) {
    if (L.first == NULL) {
        L.first = P;
        L.last = P;
    } else {
        L.last->next = P;
        P->prev = L.last;
        L.last = P;
    }
}

address findElm(List L, string nopol) {
    address temp = L.first;
    while (temp != NULL) {
        if (temp->info.nopol == nopol) {
            return temp;
        }
        temp = temp->next;
    }
    return NULL;
}

void deleteFirst(List &L, address &P) {
    P = L.first;
    if (L.first == L.last) {
        L.first = NULL;
        L.last = NULL;
    } else {
        L.first = L.first->next;
        L.first->prev = NULL;
    }
}

void deleteLast(List &L, address &P) {
    P = L.last;
    if (L.first == L.last) {
        L.first = NULL;
        L.last = NULL;
    } else {
        L.last = L.last->prev;
        L.last->next = NULL;
    }
}

void deleteAfter(address Prec, address &P) {
    P = Prec->next;
    Prec->next = P->next;
    if (P->next != NULL) {
        P->next->prev = Prec;
    }
}
```

## main.cpp

```go
#include "Doublylist.h"
#include <iostream>
using namespace std;

int main() {
    List L;
    createList(L);
    infotype data;
    address P;
    string cariNopol;

    // Data 1
    cout << "Masukkan nomor polisi: ";
    cin >> data.nopol;
    cout << "Masukkan warna kendaraan: ";
    cin >> data.warna;
    cout << "Masukkan tahun kendaraan: ";
    cin >> data.thnBuat;

    if (findElm(L, data.nopol) == NULL) {
        P = alokasi(data);
        insertLast(L, P);
    } else {
        cout << "Nomor polisi sudah terdaftar" << endl;
    }

    // Data 2
    cout << "\nMasukkan nomor polisi: ";
    cin >> data.nopol;
    cout << "Masukkan warna kendaraan: ";
    cin >> data.warna;
    cout << "Masukkan tahun kendaraan: ";
    cin >> data.thnBuat;

    if (findElm(L, data.nopol) == NULL) {
        P = alokasi(data);
        insertLast(L, P);
    } else {
        cout << "Nomor polisi sudah terdaftar" << endl;
    }

    // Data 3 - coba input duplikat
    cout << "\nMasukkan nomor polisi: ";
    cin >> data.nopol;
    cout << "Masukkan warna kendaraan: ";
    cin >> data.warna;
    cout << "Masukkan tahun kendaraan: ";
    cin >> data.thnBuat;

    if (findElm(L, data.nopol) == NULL) {
        P = alokasi(data);
        insertLast(L, P);
    } else {
        cout << "Nomor polisi sudah terdaftar" << endl;
    }

    // Data 4
    cout << "\nMasukkan nomor polisi: ";
    cin >> data.nopol;
    cout << "Masukkan warna kendaraan: ";
    cin >> data.warna;
    cout << "Masukkan tahun kendaraan: ";
    cin >> data.thnBuat;

    if (findElm(L, data.nopol) == NULL) {
        P = alokasi(data);
        insertLast(L, P);
    } else {
        cout << "Nomor polisi sudah terdaftar" << endl;
    }

    // Tampilkan semua data
    printInfo(L);

    // Cari data
    cout << "Masukkan Nomor Polisi yang dicari : ";
    cin >> cariNopol;
    address ketemu = findElm(L, cariNopol);
    if (ketemu != NULL) {
        cout << "Nomor Polisi : " << ketemu->info.nopol << endl;
        cout << "warna : " << ketemu->info.warna << endl;
        cout << "Tahun : " << ketemu->info.thnBuat << endl;
    } else {
        cout << "Data tidak ditemukan" << endl;
    }

    // Hapus data
    cout << "\nMasukkan Nomor Polisi yang akan dihapus : ";
    cin >> cariNopol;
    address hapus = findElm(L, cariNopol);
    if (hapus != NULL) {
        address P;
        if (hapus == L.first) {
            deleteFirst(L, P);
        } else if (hapus == L.last) {
            deleteLast(L, P);
        } else {
            deleteAfter(hapus->prev, P);
        }
        dealokasi(hapus);
        cout << "Data dengan nomor polisi " << cariNopol << " berhasil dihapus." << endl;
    } else {
        cout << "Data tidak ditemukan" << endl;
    }

    // Tampilkan data setelah hapus
    printInfo(L);

    return 0;
}
```

> Output
> ![Screenshot bagian x](Output/Output_no1.png)

Kode program ini mengimplementasikan Doubly Linked List untuk mengelola data kendaraan yang terdiri dari nomor polisi, warna, dan tahun pembuatan, dimana program memungkinkan pengguna untuk menambahkan data baru dengan pengecekan duplikasi nomor polisi, menampilkan seluruh data dalam format terstruktur, mencari data berdasarkan nomor polisi, serta menghapus data dengan menggunakan tiga metode penghapusan yang berbeda (deleteFirst, deleteLast, deleteAfter) yang disesuaikan berdasarkan posisi node dalam list, di mana setiap elemen memiliki pointer prev dan next sehingga memungkinkan traversing dua arah dan operasi insert/delete yang lebih efisien dibanding singly linked list.

## Referensi
1. https://www.w3schools.com/dsa/dsa_theory_linkedlists.php
2. https://www.w3schools.com/dsa/dsa_data_linkedlists_types.php
3. https://www.w3schools.com/dsa/dsa_algo_linkedlists_operations.php
4. https://www.w3schools.com/dsa/dsa_theory_linkedlists_memory.php
5. https://www.w3schools.com/dsa/dsa_examples.php
