# <h1 align="center">Laporan Praktikum Modul 13 <br>MULTI LINKED LIST</h1>
<p align="center">RIZKI WIDODO - 103112400136</p>

## Dasar Teori
Multi Linked List merupakan struktur data yang terdiri dari beberapa linked list yang saling terhubung, di mana setiap elemen dapat menjadi bagian dari lebih dari satu list secara bersamaan. Dalam implementasinya, terdapat konsep list induk (parent list) dan list anak (child list), di mana setiap elemen pada list induk dapat menunjuk ke sebuah list anak yang terpisah. Struktur ini memungkinkan representasi data hierarkis atau relasional, seperti data pegawai yang memiliki beberapa data anak. Operasi dasar seperti insert dan delete pada multi linked list memerlukan penanganan khusus karena harus mempertimbangkan keterhubungan antar list, misalnya saat menambahkan atau menghapus elemen anak harus diketahui terlebih dahulu elemen induknya, dan penghapusan elemen induk harus diikuti penghapusan seluruh elemen anak yang terkait. Implementasi multi linked list umumnya menggunakan struktur data pointer dalam bahasa pemrograman C dengan mendefinisikan tipe data untuk elemen induk dan anak beserta prosedur-prosedur untuk alokasi, dealokasi, pencarian, serta manipulasi list.
## Guided

### Guided 1
```c++
#include <iostream>
#include <string>
using namespace std;

struct ChildNode
{
    string info;
    ChildNode *next;
};

struct ParentNode
{
    string info;
    ChildNode *childHead;
    ParentNode *next;
};

ParentNode *createParent(string info)
{
    ParentNode *newNode = new ParentNode;
    newNode->info = info;
    newNode->childHead = NULL;
    newNode->next = NULL;
    return newNode;
}

ChildNode *createChild(string info)
{
    ChildNode *newNode = new ChildNode;
    newNode->info = info;
    newNode->next = NULL;
    return newNode;
}

void insertParent(ParentNode *&head, string info)
{
    ParentNode *newNode = createParent(info);
    if (head == NULL)
    {
        head = newNode;
    }
    else
    {
        ParentNode *temp = head;
        while (temp->next != NULL)
        {
            temp = temp->next;
        }
        temp->next = newNode;
    }
}

void insertChild(ParentNode *head, string parentInfo, string childInfo)
{
    ParentNode *p = head;
    while (p != NULL && p->info != parentInfo)
    {
        p = p->next;
    }
    
    if (p != NULL)
    {
        ChildNode *newChild = createChild(childInfo);
        
        if (p->childHead == NULL)
        {
            p->childHead = newChild;
        }
        else
        {
            ChildNode *c = p->childHead;
            while (c->next != NULL)
            {
                c = c->next;
            }
            c->next = newChild;
        }
    }
}

void printAll(ParentNode *head)
{
    ParentNode *p = head;
    while (p != NULL)
    {
        cout << p->info;
        ChildNode *c = p->childHead;
        if (c != NULL)
        {
            while (c != NULL)
            {
                cout << " -> " << c->info;
                c = c->next;
            }
        }
     cout << endl;
        p = p->next;
    }
}

int main()
{
    ParentNode *list = NULL;
    
    insertParent(list, "Parent Node 1");
    insertParent(list, "Parent Node 2");
    
    printAll(list);
    cout << "\n";
    
    insertChild(list, "Parent Node 1", "Child Node A");
    insertChild(list, "Parent Node 1", "Child Node B");
    insertChild(list, "Parent Node 2", "Child Node C");
    
    printAll(list);
    
    return 0;
}
```

> Output
> 
> ![Screenshot bagian x](Output/Guided1.png)
Program ini mendemonstrasikan konsep dasar Multi Linked List dengan dua node induk (parent) yang masing-masing memiliki node anak (child) terpisah. Setiap node induk merepresentasikan entitas utama (misalnya pegawai/departemen) yang dapat memiliki beberapa entitas terkait (anak). Struktur ini menggambarkan hubungan one-to-many di mana satu induk dapat merujuk ke beberapa anak.
>
> 
## UNGUIDED 
1. Perhatikan program 46 multilist.h, buat multilist.cpp untuk implementasi semua fungsi pada 
multilist.h. Buat main.cpp untuk pemanggilan fungsi-fungsi tersebut.

#### multilist.h
```c++
#ifndef MULTILIST_H_INCLUDED
#define MULTILIST_H_INCLUDED

#include <iostream>
#include <string>
using namespace std;

#define Nil NULL

typedef string infotypeanak;   // Menggunakan string untuk nama anak
typedef int infotypeinduk;     // Menggunakan int untuk ID pegawai
typedef bool boolean;

// Struktur untuk list anak
struct element_list_anak {
    infotypeanak info;
    element_list_anak* next;
    element_list_anak* prev;
};

struct listanak {
    element_list_anak* first;
    element_list_anak* last;
};

// Struktur untuk list induk
struct element_list_induk {
    infotypeinduk info;
    listanak lanak;
    element_list_induk* next;
    element_list_induk* prev;
};

struct listinduk {
    element_list_induk* first;
    element_list_induk* last;
};

typedef element_list_induk* address;
typedef element_list_anak* address_anak;

// Deklarasi fungsi
boolean ListEmpty(listinduk L);
boolean ListEmptyAnak(listanak L);

void CreateList(listinduk &L);
void CreateListAnak(listanak &L);

address alokasi(infotypeinduk X);
address_anak alokasianak(infotypeanak X);
void dealokasi(address P);
void dealokasianak(address_anak P);

address findElm(listinduk L, infotypeinduk X);
address_anak findElmAnak(listanak Lanak, infotypeanak X);

void insertFirst(listinduk &L, address P);
void insertAfter(listinduk &L, address P, address Prec);
void insertLast(listinduk &L, address P);

void insertFirstAnak(listanak &L, address_anak P);
void insertAfterAnak(listanak &L, address_anak P, address_anak Prec);
void insertLastAnak(listanak &L, address_anak P);

void delFirst(listinduk &L, address &P);
void delLast(listinduk &L, address &P);
void delAfter(listinduk &L, address &P, address Prec);
void delP(listinduk &L, infotypeinduk X);

void delFirstAnak(listanak &L, address_anak &P);
void delLastAnak(listanak &L, address_anak &P);
void delAfterAnak(listanak &L, address_anak &P, address_anak Prec);
void delPAnak(listanak &L, infotypeanak X);

void printInfo(listinduk L);
int nbList(listinduk L);
void printInfoAnak(listanak Lanak);
int nbListAnak(listanak Lanak);

void delAll(listinduk &L);

#endif 
```
#### multilist.cpp
```c++
#include "multilist.h"

// ===== FUNGSI PENGECEKAN LIST KOSONG =====
boolean ListEmpty(listinduk L) {
    return (L.first == Nil);
}

boolean ListEmptyAnak(listanak L) {
    return (L.first == Nil);
}

// ===== FUNGSI PEMBUATAN LIST KOSONG =====
void CreateList(listinduk &L) {
    L.first = Nil;
    L.last = Nil;
}

void CreateListAnak(listanak &L) {
    L.first = Nil;
    L.last = Nil;
}

// ===== FUNGSI ALOKASI & DEALOKASI =====
address alokasi(infotypeinduk X) {
    address P = new element_list_induk;
    if(P != Nil) {
        P->info = X;
        CreateListAnak(P->lanak);
        P->next = Nil;
        P->prev = Nil;
    }
    return P;
}

address_anak alokasianak(infotypeanak X) {
    address_anak P = new element_list_anak;
    if(P != Nil) {
        P->info = X;
        P->next = Nil;
        P->prev = Nil;
    }
    return P;
}

void dealokasi(address P) {
    delete P;
}

void dealokasianak(address_anak P) {
    delete P;
}

// ===== FUNGSI PENCARIAN ELEMEN =====
address findElm(listinduk L, infotypeinduk X) {
    address P = L.first;
    while(P != Nil) {
        if(P->info == X) return P;
        P = P->next;
    }
    return Nil;
}

address_anak findElmAnak(listanak Lanak, infotypeanak X) {
    address_anak P = Lanak.first;
    while(P != Nil) {
        if(P->info == X) return P;
        P = P->next;
    }
    return Nil;
}

// ===== FUNGSI INSERT UNTUK INDUK =====
void insertFirst(listinduk &L, address P) {
    if(L.first == Nil) {
        L.first = P;
        L.last = P;
    } else {
        P->next = L.first;
        L.first->prev = P;
        L.first = P;
    }
}

void insertAfter(listinduk &L, address P, address Prec) {
    if(Prec != Nil) {
        P->next = Prec->next;
        P->prev = Prec;
        if(Prec->next != Nil) {
            Prec->next->prev = P;
        } else {
            L.last = P;
        }
        Prec->next = P;
    }
}

void insertLast(listinduk &L, address P) {
    if(L.first == Nil) {
        L.first = P;
        L.last = P;
    } else {
        P->prev = L.last;
        L.last->next = P;
        L.last = P;
    }
}

// ===== FUNGSI INSERT UNTUK ANAK =====
void insertFirstAnak(listanak &L, address_anak P) {
    if(L.first == Nil) {
        L.first = P;
        L.last = P;
    } else {
        P->next = L.first;
        L.first->prev = P;
        L.first = P;
    }
}

void insertAfterAnak(listanak &L, address_anak P, address_anak Prec) {
    if(Prec != Nil) {
        P->next = Prec->next;
        P->prev = Prec;
        if(Prec->next != Nil) {
            Prec->next->prev = P;
        } else {
            L.last = P;
        }
        Prec->next = P;
    }
}

void insertLastAnak(listanak &L, address_anak P) {
    if(L.first == Nil) {
        L.first = P;
        L.last = P;
    } else {
        P->prev = L.last;
        L.last->next = P;
        L.last = P;
    }
}

// ===== FUNGSI DELETE UNTUK INDUK =====
void delFirst(listinduk &L, address &P) {
    if(L.first != Nil) {
        P = L.first;
        if(L.first == L.last) {
            L.first = Nil;
            L.last = Nil;
        } else {
            L.first = L.first->next;
            L.first->prev = Nil;
        }
        P->next = Nil;
        P->prev = Nil;
    }
}

void delLast(listinduk &L, address &P) {
    if(L.last != Nil) {
        P = L.last;
        if(L.first == L.last) {
            L.first = Nil;
            L.last = Nil;
        } else {
            L.last = L.last->prev;
            L.last->next = Nil;
        }
        P->next = Nil;
        P->prev = Nil;
    }
}

void delAfter(listinduk &L, address &P, address Prec) {
    if(Prec != Nil && Prec->next != Nil) {
        P = Prec->next;
        Prec->next = P->next;
        if(P->next != Nil) {
            P->next->prev = Prec;
        } else {
            L.last = Prec;
        }
        P->next = Nil;
        P->prev = Nil;
    }
}

void delP(listinduk &L, infotypeinduk X) {
    address P = findElm(L, X);
    if(P != Nil) {
        // Hapus semua anak terlebih dahulu
        address_anak A;
        while(P->lanak.first != Nil) {
            delFirstAnak(P->lanak, A);
            dealokasianak(A);
        }
        
        // Hapus induk dari list
        if(P == L.first) {
            delFirst(L, P);
        } else if(P == L.last) {
            delLast(L, P);
        } else {
            P->prev->next = P->next;
            P->next->prev = P->prev;
            P->next = Nil;
            P->prev = Nil;
        }
        dealokasi(P);
    }
}

// ===== FUNGSI DELETE UNTUK ANAK =====
void delFirstAnak(listanak &L, address_anak &P) {
    if(L.first != Nil) {
        P = L.first;
        if(L.first == L.last) {
            L.first = Nil;
            L.last = Nil;
        } else {
            L.first = L.first->next;
            L.first->prev = Nil;
        }
        P->next = Nil;
        P->prev = Nil;
    }
}

void delLastAnak(listanak &L, address_anak &P) {
    if(L.last != Nil) {
        P = L.last;
        if(L.first == L.last) {
            L.first = Nil;
            L.last = Nil;
        } else {
            L.last = L.last->prev;
            L.last->next = Nil;
        }
        P->next = Nil;
        P->prev = Nil;
    }
}

void delAfterAnak(listanak &L, address_anak &P, address_anak Prec) {
    if(Prec != Nil && Prec->next != Nil) {
        P = Prec->next;
        Prec->next = P->next;
        if(P->next != Nil) {
            P->next->prev = Prec;
        } else {
            L.last = Prec;
        }
        P->next = Nil;
        P->prev = Nil;
    }
}

void delPAnak(listanak &L, infotypeanak X) {
    address_anak P = findElmAnak(L, X);
    if(P != Nil) {
        if(P == L.first) {
            delFirstAnak(L, P);
        } else if(P == L.last) {
            L.last = P->prev;
            if(L.last != Nil) {
                L.last->next = Nil;
            }
        } else {
            P->prev->next = P->next;
            P->next->prev = P->prev;
        }
        dealokasianak(P);
    }
}

// ===== FUNGSI OUTPUT & PENGHITUNGAN =====
void printInfo(listinduk L) {
    address P = L.first;
    while(P != Nil) {
        cout << "[Pegawai ID: " << P->info << "] -> Anak: ";
        
        address_anak Q = P->lanak.first;
        if(Q == Nil) {
            cout << "(Tidak ada anak)";
        } else {
            while(Q != Nil) {
                cout << Q->info;
                if(Q->next != Nil) cout << ", ";
                Q = Q->next;
            }
        }
        cout << endl;
        P = P->next;
    }
}

int nbList(listinduk L) {
    int count = 0;
    address P = L.first;
    while(P != Nil) {
        count++;
        P = P->next;
    }
    return count;
}

void printInfoAnak(listanak Lanak) {
    address_anak P = Lanak.first;
    while(P != Nil) {
        cout << P->info << " ";
        P = P->next;
    }
    cout << endl;
}

int nbListAnak(listanak Lanak) {
    int count = 0;
    address_anak P = Lanak.first;
    while(P != Nil) {
        count++;
        P = P->next;
    }
    return count;
}

// ===== FUNGSI HAPUS SEMUA =====
void delAll(listinduk &L) {
    address P;
    while(L.first != Nil) {
        // Hapus semua anak dari induk pertama
        address_anak A;
        while(L.first->lanak.first != Nil) {
            delFirstAnak(L.first->lanak, A);
            dealokasianak(A);
        }
        
        // Hapus induk
        P = L.first;
        L.first = L.first->next;
        dealokasi(P);
    }
    L.last = Nil;
}
```
#### main.cpp
```c++
#include "multilist.h"
#include <iostream>
using namespace std;

int main() {
    listinduk L;
    CreateList(L);
    
    cout << "=== DEMO MULTI LINKED LIST ===" << endl << endl;
    
    // 1. Tambahkan 3 pegawai (induk)
    cout << "1. Menambahkan 3 pegawai..." << endl;
    address peg1 = alokasi(101);
    insertLast(L, peg1);
    
    address peg2 = alokasi(102);
    insertLast(L, peg2);
    
    address peg3 = alokasi(103);
    insertLast(L, peg3);
    cout << "   Pegawai 101, 102, 103 ditambahkan" << endl << endl;
    
    // 2. Tambahkan anak untuk pegawai 101
    cout << "2. Menambahkan anak untuk Pegawai 101..." << endl;
    address_anak anak1 = alokasianak("Budi");
    insertLastAnak(peg1->lanak, anak1);
    
    address_anak anak2 = alokasianak("Ari");
    insertLastAnak(peg1->lanak, anak2);
    cout << "   Anak Budi dan Ari ditambahkan" << endl << endl;
    
    // 3. Tambahkan anak untuk pegawai 102
    cout << "3. Menambahkan anak untuk Pegawai 102..." << endl;
    address_anak anak3 = alokasianak("Siti");
    insertLastAnak(peg2->lanak, anak3);
    cout << "   Anak Siti ditambahkan" << endl << endl;
    
    // 4. Tampilkan data awal
    cout << "Data Awal:" << endl;
    printInfo(L);
    cout << endl;
    
    // 5. Hapus anak 'Budi' dari pegawai 101
    cout << "Menghapus anak 'Budi' dari Pegawai 101..." << endl;
    delPAnak(peg1->lanak, "Budi");
    printInfo(L);
    cout << endl;
    
    // 6. Hapus pegawai 102 (beserta anaknya)
    cout << "Menghapus Pegawai 102 (beserta anaknya 'Siti')..." << endl;
    delP(L, 102);
    printInfo(L);
    cout << endl;
    
    // 7. Hapus semua data
    cout << "Menghapus semua data dari memory..." << endl;
    delAll(L);
    if(ListEmpty(L)) {
        cout << "Semua data berhasil dihapus" << endl;
    }
    
    cout << "\n=== PROGRAM SELESAI ===" << endl;
    return 0;
}
```
> Output soal 2
> 
> ![Screenshot bagian x](Output/soal2.png)
Program Multi Linked List yang telah diimplementasikan menampilkan hubungan hierarkis antara data induk (pegawai) dan anak-anaknya, di mana setiap pegawai direpresentasikan sebagai node induk dengan list anak terpisah. Dalam contoh eksekusi, Pegawai ID 101 memiliki anak Budi dan Ari, Pegawai 102 memiliki anak Siti, dan Pegawai 103 tanpa anak. Operasi penghapusan anak Budi hanya menghapus node anak tersebut dari list anak milik Pegawai 101, sementara node induk dan anak lainnya tetap utuh. Sebaliknya, penghapusan Pegawai 102 secara otomatis menghapus seluruh list anak yang terhubung (termasuk Siti) sebelum menghapus node induknya, menunjukkan konsistensi logika penghapusan dalam Multi Linked List yang menjaga integritas struktur dan mencegah memory leak.

2. Implementasikan ADT Multi Linked List dan ADT Linked List untuk studi kasus data mahasiswa (Nama, NIM, Jenis Kelamin, IPK) beserta seluruh operasi manipulasi data (insert, delete, find, print) menggunakan bahasa C++ sesuai spesifikasi header yang diberikan.

#### multilist.h
```c++
#ifndef CIRCULARLIST_H_INCLUDED
#define CIRCULARLIST_H_INCLUDED

#include <iostream>
#include <string>
#define Nil NULL

using namespace std;

struct mahasiswa {
    string nama;
    string nim;
    char jenis_kelamin;
    float ipk;
};

typedef mahasiswa infotype;
typedef struct ElmList *address;

struct ElmList {
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
void dealokasi(address P);
void insertFirst(List &L, address P);
void insertLast(List &L, address P);
void insertAfter(List &L, address Prec, address P);
void deleteFirst(List &L, address &P);
void deleteLast(List &L, address &P);
void deleteAfter(List &L, address Prec, address &P);
address findElm(List L, string nim);
void printInfo(List L);

address createData(string nama, string nim, char jenis_kelamin, float ipk);

#endif
```
#### multi list.cpp
```c++
#include "circularlist.h"

void createList(List &L) {
    L.first = Nil;
    L.last = Nil;
}

address alokasi(infotype x) {
    address P = new ElmList;
    if (P != Nil) {
        P->info = x;
        P->next = Nil;
        P->prev = Nil;
    }
    return P;
}

address createData(string nama, string nim, char jenis_kelamin, float ipk) {
    infotype x;
    address P;
    x.nama = nama;
    x.nim = nim;
    x.jenis_kelamin = jenis_kelamin;
    x.ipk = ipk;
    P = alokasi(x);
    return P;
}

void dealokasi(address P) {
    delete P;
}

void insertFirst(List &L, address P) {
    if (L.first == Nil) {
        L.first = P;
        L.last = P;
        P->next = P;
        P->prev = P;
    } else {
        P->next = L.first;
        P->prev = L.last;
        L.first->prev = P;
        L.last->next = P;
        L.first = P;
    }
}

void insertLast(List &L, address P) {
    if (L.first == Nil) {
        L.first = P;
        L.last = P;
        P->next = P;
        P->prev = P;
    } else {
        P->next = L.first;
        P->prev = L.last;
        L.last->next = P;
        L.first->prev = P;
        L.last = P;
    }
}

void insertAfter(List &L, address Prec, address P) {
    if (Prec != Nil) {
        P->next = Prec->next;
        P->prev = Prec;
        Prec->next->prev = P;
        Prec->next = P;
        
        if (Prec == L.last) {
            L.last = P;
        }
    }
}

void deleteFirst(List &L, address &P) {
    P = L.first;
    if (P != Nil) {
        if (L.first == L.last) { 
            L.first = Nil;
            L.last = Nil;
        } else {
            L.first = P->next;
            L.first->prev = L.last;
            L.last->next = L.first;
        }
        P->next = Nil;
        P->prev = Nil;
    }
}

void deleteLast(List &L, address &P) {
    P = L.last;
    if (P != Nil) {
        if (L.first == L.last) {
            L.first = Nil;
            L.last = Nil;
        } else {
            L.last = P->prev;
            L.last->next = L.first;
            L.first->prev = L.last;
        }
        P->next = Nil;
        P->prev = Nil;
    }
}

void deleteAfter(List &L, address Prec, address &P) {
    if (Prec != Nil && Prec->next != Prec) { 
        P = Prec->next;
        Prec->next = P->next;
        P->next->prev = Prec;
        
        if (P == L.last) {
            L.last = Prec;
        } else if (P == L.first) {
            L.first = P->next;
        }
        
        P->next = Nil;
        P->prev = Nil;
    }
}

address findElm(List L, string nim) {
    if (L.first == Nil) return Nil;
    
    address P = L.first;
    do {
        if (P->info.nim == nim) return P;
        P = P->next;
    } while (P != L.first); 
    
    return Nil;
}

void printInfo(List L) {
    if (L.first == Nil) {
        cout << "List Kosong" << endl;
    } else {
        address P = L.first;
        do {
            cout << "Nama : " << P->info.nama << endl;
            cout << "NIM  : " << P->info.nim << endl;
            cout << "L/P  : " << P->info.jenis_kelamin << endl;
            cout << "IPK  : " << P->info.ipk << endl;
            cout << endl;
            P = P->next;
        } while (P != L.first); 
}
```
#### main.cpp
```c++
#include <iostream>
#include "circularlist.h"

using namespace std;

int main() {
    List L;
    address P1 = Nil;
    address P2 = Nil;
    infotype x;
    
    createList(L);
    cout << "coba insert first, last, dan after" << endl;
    
    P1 = createData("Danu", "04", 'l', 4);
    insertFirst(L, P1);
    
    P1 = createData("Fahmi", "06", 'l', 3.45);
    insertLast(L, P1);
    
    P1 = createData("Bobi", "02", 'l', 3.71);
    insertFirst(L, P1);
    
    P1 = createData("Ali", "01", 'l', 3.3);
    insertFirst(L, P1);
    
    P1 = createData("Gita", "07", 'p', 3.75);
    insertLast(L, P1);
    
    x.nim = "02"; 
    P1 = findElm(L, x.nim);
    if(P1 != Nil) {
        P2 = createData("Cindi", "03", 'p', 3.5);
        insertAfter(L, P1, P2);
    }
    
    x.nim = "04"; 
    P1 = findElm(L, x.nim);
    if(P1 != Nil) {
        P2 = createData("Eli", "05", 'p', 3.4);
        insertAfter(L, P1, P2);
    }
    
    x.nim = "07"; 
    P1 = findElm(L, x.nim);
    if(P1 != Nil) {
        P2 = createData("Hilmi", "08", 'l', 3.3);
        insertAfter(L, P1, P2);
    }
    
    printInfo(L);
    
    return 0;
}
```

> Output soal 2
>
> ![Screenshot bagian x](OUTPUT/unguided2.png)
Circular Linked List yang digunakan untuk mengelola data mahasiswa. Program berhasil melakukan operasi insert first, insert last, dan insert after untuk menambahkan data mahasiswa seperti Ali, Bobi, Cindi, Danu, Eli, dan seterusnya. Setiap data menampilkan atribut Nama, NIM, Jenis Kelamin, dan IPK. Hasil ini membuktikan bahwa struktur Circular Linked List bekerja dengan baik untuk menyisipkan data di berbagai posisi dalam list dan membentuk siklus data yang saling terhubung.


## Referensi

1. https://www.geeksforgeeks.org/multilist-in-data-structure/ (diakses pada 8 Desember 2025)
2. https://www.tutorialspoint.com/data_structures_algorithms/linked_list_algorithms.htm (diakses pada 8 Desember 2025)
3. https://www.programiz.com/dsa/linked-list (diakses pada 8 Desember 2025)
