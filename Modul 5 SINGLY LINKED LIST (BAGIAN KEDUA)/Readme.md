
# <h1 align="center">Laporan Praktikum Modul 5 <br> SINGLY LINKED LIST (BAGIAN KEDUA)</h1>
<p align="center">Rizki Widodo - 103112400136 </p>

## Dasar Teori

Teori Dasar Singly Linked List
Singly Linked List adalah struktur data linier yang terdiri dari serangkaian node yang setiap node-nya menyimpan data (infotype) dan pointer ke node berikutnya (next). Berbeda dengan array, linked list menggunakan alokasi memori dinamis sehingga ukurannya dapat bertambah atau berkurang selama program berjalan. Operasi dasar pada singly linked list meliputi penambahan node (insertFirst, insertLast, insertAfter), penghapusan node (delFirst, delLast, delAfter), pencarian elemen (findElm), dan traversal untuk menampilkan atau memproses data. Keunggulan utamanya adalah fleksibilitas dalam manajemen memori dan efisiensi dalam operasi penambahan/penghapusan elemen, meskipun akses elemen harus dilakukan secara sekuensial.
## Guided

### LINKEDLIST.CPP
```go
#include <iostream>
using namespace std;

// Struktur Node
struct Node {
    int data;
    Node* next;
};

// Pointer awal dan akhir
Node* head = nullptr;

// Fungsi untuk membuat node baru
Node* createNode(int data) {
    Node* newNode = new Node();
    newNode->data = data;
    newNode->next = nullptr;
    return newNode;
}


void insertBelakang(int data) {
    Node* newNode = createNode(data);
    if (head == nullptr) {
        head = newNode;
    } else {
        Node* temp = head;
        while (temp->next != nullptr) {
            temp = temp->next;
        }
        temp->next = newNode;
    }
    cout << "Data " << data << " berhasil ditambahkan di belakang.\n";
}

void insertSetelah(int target, int dataBaru) {
    Node* temp = head;
    while (temp != nullptr && temp->data != target) {
        temp = temp->next;
    }

    if (temp == nullptr) {
        cout << "Data " << target << " tidak ditemukan!\n";
    } else {
        Node* newNode = createNode(dataBaru);
        newNode->next = temp->next;
        temp->next = newNode;
        cout << "Data " << dataBaru << " berhasil disisipkan setelah " << target << ".\n";
    }
}

// ========== DELETE FUNCTION ==========
void hapusNode(int data) {
    if (head == nullptr) {
        cout << "List kosong!\n";
        return;
    }

    Node* temp = head;
    Node* prev = nullptr;

    // Jika data di node pertama
    if (temp != nullptr && temp->data == data) {
        head = temp->next;
        delete temp;
        cout << "Data " << data << " berhasil dihapus.\n";
        return;
    }

    // Cari node yang akan dihapus
    while (temp != nullptr && temp->data != data) {
        prev = temp;
        temp = temp->next;
    }

    // Jika data tidak ditemukan
    if (temp == nullptr) {
        cout << "Data " << data << " tidak ditemukan!\n";
        return;
    }

    prev->next = temp->next;
    delete temp;
    cout << "Data " << data << " berhasil dihapus.\n";
}

// ========== UPDATE FUNCTION ==========
void updateNode(int dataLama, int dataBaru) {
    Node* temp = head;
    while (temp != nullptr && temp->data != dataLama) {
        temp = temp->next;
    }

    if (temp == nullptr) {
        cout << "Data " << dataLama << " tidak ditemukan!\n";
    } else {
        temp->data = dataBaru;
        cout << "Data " << dataLama << " berhasil diupdate menjadi " << dataBaru << ".\n";
    }
}

// ========== DISPLAY FUNCTION ==========
void tampilkanList() {
    if (head == nullptr) {
        cout << "List kosong!\n";
        return;
    }

    Node* temp = head;
    cout << "Isi Linked List: ";
    while (temp != nullptr) {
        cout << temp->data << " -> ";
        temp = temp->next
    }
    cout << "NULL\n";
}

// ========== MAIN PROGRAM ==========
int main() {
    int pilihan, data, target, dataBaru;

    do {
        cout << "\n=== MENU SINGLE LINKED LIST ===\n";
        cout << "1. Insert Depan\n";
        cout << "2. Insert Belakang\n";
        cout << "3. Insert Setelah\n";
        cout << "4. Hapus Data\n";
        cout << "5. Update Data\n";
        cout << "6. Tampilkan List\n";
        cout << "0. Keluar\n";
        cout << "Pilih: ";
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
                cin >> dataBaru;
                insertSetelah(target, dataBaru);
                break;
            case 4:
                cout << "Masukkan data yang ingin dihapus: ";
                cin >> data;
                hapusNode(data);
                break;
            case 5:
                cout << "Masukkan data lama: ";
                cin >> data;
                cout << "Masukkan data baru: ";
                cin >> dataBaru;
                updateNode(data, dataBaru);
                break;
            case 6:
                tampilkanList();
                break;
            case 0:
                cout << "Program selesai.\n";
                break;
            default:
                cout << "Pilihan tidak valid!\n";
        }
    } while (pilihan != 0);

    return 0;
}
```

### SEARCHING.CPP
```go
#include <iostream>
using namespace std;

#define Nil nullptr

// Deklarasi struktur Node
struct Node {
    int info;
    Node* next;

    Node(int value) {
        info = value;
        next = Nil;
    }
};

// Kelas List
class List {
private:
    Node* first;

public:
    // Konstruktor
    List() {
        first = Nil;
    }

    // Mengecek apakah list kosong
    bool isEmpty() {
        return first == Nil;
    }

    // Membuat list kosong
    void createList() {
        first = Nil;
    }

    // Menampilkan isi list
    void printInfo() {
        if (isEmpty()) {
            cout << "List kosong" << endl;
        } else {
            Node* p = first;
            cout << "Isi list: ";
            while (p != Nil) {
                cout << p->info << " ";
                p = p->next;
            }
            cout << endl;
        }
    }

    // Menghitung jumlah elemen
    int nbList() {
        int count = 0;
        Node* p = first;
        while (p != Nil) {
            count++;
            p = p->next;
        }
        return count;
    }

    // Menyisipkan elemen di awal
    void insertFirst(int value) {
        Node* p = new Node(value);
        p->next = first;
        first = p;
    }

    // Menyisipkan elemen di akhir
    void insertLast(int value) {
        Node* p = new Node(value);
        if (isEmpty()) {
            first = p;
        } else {
            Node* last = first;
            while (last->next != Nil) {
                last = last->next;
            }
            last->next = p;
        }
    }

    // Menyisipkan elemen setelah node tertentu
    void insertAfter(Node* prec, int value) {
        if (prec != Nil) {
            Node* p = new Node(value);
            p->next = prec->next;
            prec->next = p;
        }
    }

    // Menghapus elemen pertama
    void delFirst() {
        if (!isEmpty()) {
            Node* temp = first;
            first = first->next;
            delete temp;
        }
    }

    // Menghapus elemen terakhir
    void delLast() {
        if (!isEmpty()) {
            if (first->next == Nil) {
                delete first;
                first = Nil;
            } else {
                Node* prev = Nil;
                Node* curr = first;
                while (curr->next != Nil) {
                    prev = curr;
                    curr = curr->next;
                }
                prev->next = Nil;
                delete curr;
            }
        }
    }

    // Menghapus elemen setelah node tertentu
    void delAfter(Node* prec) {
        if (prec != Nil && prec->next != Nil) {
            Node* temp = prec->next;
            prec->next = temp->next;
            delete temp;
        }
    }

    // Menghapus elemen dengan nilai tertentu
    void delP(int value) {
        if (isEmpty()) return;

        Node* curr = first;
        Node* prev = Nil;

        while (curr != Nil && curr->info != value) {
            prev = curr;
            curr = curr->next;
        }

        if (curr != Nil) { // ditemukan
            if (prev == Nil)
                first = curr->next;
            else
                prev->next = curr->next;
            delete curr;
        }
    }

    // Mencari elemen dengan nilai tertentu
    Node* findElm(int value) {
        Node* p = first;
        while (p != Nil) {
            if (p->info == value)
                return p;
            p = p->next;
        }
        return Nil;
    }

    // Membalik urutan elemen list
    void invertList() {
        Node* prev = Nil;
        Node* curr = first;
        Node* next = Nil;

        while (curr != Nil) {
            next = curr->next;
            curr->next = prev;
            prev = curr;
            curr = next;
        }
        first = prev;
    }

    // Menghapus semua elemen list
    void delAll() {
        Node* p = first;
        while (p != Nil) {
            Node* temp = p;
            p = p->next;
            delete temp;
        }
        first = Nil;
    }

    // Menyalin isi list ke list lain
    void copyList(List &L2) {
        delAll();
        Node* p = first;
        while (p != Nil) {
            L2.insertLast(p->info);
            p = p->next;
        }
    }

    // Destruktor
    ~List() {
        delAll();
    }
};

// Contoh penggunaan dalam main
int main() {
    List L;

    cout << "=== Program Linked List C++ ===" << endl;

    L.insertFirst(10);
    L.insertFirst(5);
    L.insertLast(15);
    L.insertLast(20);

    L.printInfo(); // Output: 5 10 15 20
    cout << "Jumlah elemen: " << L.nbList() << endl;

    cout << "Hapus elemen pertama..." << endl;
    L.delFirst();
    L.printInfo();

    cout << "Hapus elemen terakhir..." << endl;
    L.delLast();
    L.printInfo();

    cout << "Cari elemen bernilai 10..." << endl;
    Node* found = L.findElm(10);
    if (found) cout << "Ditemukan: " << found->info << endl;
    else cout << "Tidak ditemukan" << endl;

    cout << "Balik urutan list..." << endl;
    L.invertList();
    L.printInfo();

    cout << "Hapus semua elemen..." << endl;
    L.delAll();
    L.printInfo();

    return 0;
}
```

## Unguided

### Soal 1

buatlah searcing untuk mencari nama pembeli pada unguided sebelumnya

```go
#include <iostream>
#include <string>
using namespace std;

struct Pembeli {
    string nama;
    string pesanan;
    Pembeli* next;
};

Pembeli* buatPembeli(string nama, string pesanan) {
    Pembeli* baru = new Pembeli();
    baru->nama = nama;
    baru->pesanan = pesanan;
    baru->next = nullptr;
    return baru;
}

void tambahAntrian(Pembeli* &head, Pembeli* &tail, string nama, string pesanan) {
    Pembeli* baru = buatPembeli(nama, pesanan);
    
    if (head == nullptr) {
        head = baru;
        tail = baru;
    } else {
        tail->next = baru;
        tail = baru;
    }
    cout << "Pembeli " << nama << " telah ditambahkan ke antrian.\n";
}

void layaniAntrian(Pembeli* &head, Pembeli* &tail) {
    if (head == nullptr) {
        cout << "Antrian kosong! Tidak ada yang dilayani.\n";
        return;
    }
    
    Pembeli* hapus = head;
    cout << "Melayani: " << head->nama << " - Pesanan: " << head->pesanan << endl;
    
    head = head->next;
    
    if (head == nullptr) {
        tail = nullptr;
    }
    
    delete hapus;
}

// FUNGSI SEARCHING UNTUK MENCARI NAMA PEMBELI
void cariPembeli(Pembeli* head, string namaCari) {
    Pembeli* current = head;
    bool ditemukan = false;
    int posisi = 1;
    
    cout << "\n=== HASIL PENCARIAN ===" << endl;
    
    while (current != nullptr) {
        if (current->nama == namaCari) {
            cout << "Pembeli DITEMUKAN!" << endl;
            cout << "Posisi antrian: " << posisi << endl;
            cout << "Nama: " << current->nama << endl;
            cout << "Pesanan: " << current->pesanan << endl;
            ditemukan = true;
            break;
        }
        current = current->next;
        posisi++;
    }
    
    if (!ditemukan) {
        cout << "Pembeli dengan nama '" << namaCari << "' TIDAK DITEMUKAN dalam antrian!" << endl;
    }
    cout << "========================" << endl;
}

void tampilkanAntrian(Pembeli* head) {
    if (head == nullptr) {
        cout << "Antrian kosong!\n";
        return;
    }
    
    Pembeli* current = head;
    int counter = 1;
    
    cout << "=== DAFTAR ANTRIAN ===" << endl;
    while (current != nullptr) {
        cout << counter << ". Nama: " << current->nama 
             << " | Pesanan: " << current->pesanan << endl;
        current = current->next;
        counter++;
    }
}

void hapusSemuaAntrian(Pembeli* &head, Pembeli* &tail) {
    while (head != nullptr) {
        Pembeli* hapus = head;
        head = head->next;
        delete hapus;
    }
    tail = nullptr;
}

int main() {
    Pembeli* head = nullptr;
    Pembeli* tail = nullptr;
    int pilihan;
    string nama, pesanan, namaCari;
    
    do {
        cout << "\n=== MENU ANTRIAN ===" << endl;
        cout << "1. Tambah Antrian" << endl;
        cout << "2. Layani Antrian" << endl;
        cout << "3. Tampilkan Antrian" << endl;
        cout << "4. Cari Pembeli (Searching)" << endl;
        cout << "5. Keluar" << endl;
        cout << "Pilihan: ";
        cin >> pilihan;
        
        switch (pilihan) {
            case 1:
                cout << "Nama Pembeli: ";
                cin.ignore();
                getline(cin, nama);
                cout << "Pesanan: ";
                getline(cin, pesanan);
                tambahAntrian(head, tail, nama, pesanan);
                break;
                
            case 2:
                layaniAntrian(head, tail);
                break;
                
            case 3:
                tampilkanAntrian(head);
                break;
                
            case 4:
                cout << "Masukkan nama pembeli yang dicari: ";
                cin.ignore();
                getline(cin, namaCari);
                cariPembeli(head, namaCari);
                break;
                
            case 5:
                hapusSemuaAntrian(head, tail);
                cout << "Program selesai.\n";
                break;
                
            default:
                cout << "Pilihan tidak valid!\n";
        }
    } while (pilihan != 5);
    
    return 0;
}
```

> Output
> ![Screenshot bagian x](https://github.com/Nashiw/Laporan-Praktikum/blob/main/Modul%205/jawaban%201.png)

Program antrian pembeli ini menggunakan struktur data linked list untuk mengelola antrian dengan sistem FIFO (First In First Out) dimana pembeli yang pertama masuk akan dilayani pertama kali. Program memiliki fitur tambah antrian di belakang, layani antrian di depan, tampilkan seluruh antrian, dan fitur pencarian (searching) yang dapat mencari pembeli berdasarkan nama dengan cara menelusuri setiap node secara berurutan dari depan ke belakang hingga data ditemukan atau sampai akhir antrian. Jika pembeli ditemukan, program akan menampilkan posisi antrian dan detail pesanan, sedangkan jika tidak ditemukan akan memberikan pemberitahuan yang sesuai.
 


### Soal 2

gunakan latihan pada pertemuan minggun ini dan tambahkan seardhing untuk mencari buku berdasarkan judul, penulis, dan ISBN

```go
#include <iostream>
#include <string>
using namespace std;

struct Buku {
    string isbn;
    string judul;
    string penulis;
    Buku* next;
};

Buku* buatBuku(string isbn, string judul, string penulis) {
    Buku* bukuBaru = new Buku();
    bukuBaru->isbn = isbn;
    bukuBaru->judul = judul;
    bukuBaru->penulis = penulis;
    bukuBaru->next = NULL;
    return bukuBaru;
}

void tambahBuku(Buku** head, string isbn, string judul, string penulis) {
    Buku* bukuBaru = buatBuku(isbn, judul, penulis);
    
    if (*head == NULL) {
        *head = bukuBaru;
    } else {
        Buku* temp = *head;
        while (temp->next != NULL) {
            temp = temp->next;
        }
        temp->next = bukuBaru;
    }
    cout << "Buku berhasil ditambahkan!\n";
}

void hapusBuku(Buku** head, string isbn) {
    if (*head == NULL) {
        cout << "Daftar buku kosong!\n";
        return;
    }
    
    Buku* temp = *head;
    Buku* prev = NULL;
    
    if (temp != NULL && temp->isbn == isbn) {
        *head = temp->next;
        delete temp;
        cout << "Buku berhasil dihapus!\n";
        return;
    }
    
    while (temp != NULL && temp->isbn != isbn) {
        prev = temp;
        temp = temp->next;
    }
    
    if (temp == NULL) {
        cout << "Buku tidak ditemukan!\n";
        return;
    }
    
    prev->next = temp->next;
    delete temp;
    cout << "Buku berhasil dihapus!\n";
}

void perbaruiBuku(Buku* head, string isbn) {
    if (head == NULL) {
        cout << "Daftar buku kosong!\n";
        return;
    }
    
    Buku* temp = head;
    while (temp != NULL) {
        if (temp->isbn == isbn) {
            cout << "Masukkan judul baru: ";
            cin.ignore();
            getline(cin, temp->judul);
            cout << "Masukkan penulis baru: ";
            getline(cin, temp->penulis);
            cout << "Buku berhasil diperbarui!\n";
            return;
        }
        temp = temp->next;
    }
    
    cout << "Buku tidak ditemukan!\n";
}

void lihatSemuaBuku(Buku* head) {
    if (head == NULL) {
        cout << "Daftar buku kosong!\n";
        return;
    }
    
    Buku* temp = head;
    int no = 1;
    cout << "\n=== DAFTAR BUKU ===\n";
    while (temp != NULL) {
        cout << "Buku #" << no << ":\n";
        cout << "ISBN   : " << temp->isbn << endl;
        cout << "Judul  : " << temp->judul << endl;
        cout << "Penulis: " << temp->penulis << endl;
        cout << "-------------------\n";
        temp = temp->next;
        no++;
    }
}

void lihatSatuBuku(Buku* head, string isbn) {
    if (head == NULL) {
        cout << "Daftar buku kosong!\n";
        return;
    }
    
    Buku* temp = head;
    while (temp != NULL) {
        if (temp->isbn == isbn) {
            cout << "\n=== DETAIL BUKU ===\n";
            cout << "ISBN   : " << temp->isbn << endl;
            cout << "Judul  : " << temp->judul << endl;
            cout << "Penulis: " << temp->penulis << endl;
            return;
        }
        temp = temp->next;
    }
    
    cout << "Buku tidak ditemukan!\n";
}

// FUNGSI SEARCHING BARU - CARI BERDASARKAN JUDUL
void cariBukuJudul(Buku* head, string cariJudul) {
    if (head == NULL) {
        cout << "Daftar buku kosong!\n";
        return;
    }
    
    Buku* temp = head;
    bool ditemukan = false;
    
    cout << "\n=== HASIL PENCARIAN JUDUL: " << cariJudul << " ===\n";
    while (temp != NULL) {
        if (temp->judul.find(cariJudul) != string::npos) {
            cout << "ISBN   : " << temp->isbn << endl;
            cout << "Judul  : " << temp->judul << endl;
            cout << "Penulis: " << temp->penulis << endl;
            cout << "-------------------\n";
            ditemukan = true;
        }
        temp = temp->next;
    }
    
    if (!ditemukan) {
        cout << "Buku dengan judul '" << cariJudul << "' tidak ditemukan!\n";
    }
}

// FUNGSI SEARCHING BARU - CARI BERDASARKAN PENULIS
void cariBukuPenulis(Buku* head, string cariPenulis) {
    if (head == NULL) {
        cout << "Daftar buku kosong!\n";
        return;
    }
    
    Buku* temp = head;
    bool ditemukan = false;
    
    cout << "\n=== HASIL PENCARIAN PENULIS: " << cariPenulis << " ===\n";
    while (temp != NULL) {
        if (temp->penulis.find(cariPenulis) != string::npos) {
            cout << "ISBN   : " << temp->isbn << endl;
            cout << "Judul  : " << temp->judul << endl;
            cout << "Penulis: " << temp->penulis << endl;
            cout << "-------------------\n";
            ditemukan = true;
        }
        temp = temp->next;
    }
    
    if (!ditemukan) {
        cout << "Buku dengan penulis '" << cariPenulis << "' tidak ditemukan!\n";
    }
}

// FUNGSI SEARCHING BARU - CARI BERDASARKAN ISBN
void cariBukuISBN(Buku* head, string cariISBN) {
    if (head == NULL) {
        cout << "Daftar buku kosong!\n";
        return;
    }
    
    Buku* temp = head;
    
    cout << "\n=== HASIL PENCARIAN ISBN: " << cariISBN << " ===\n";
    while (temp != NULL) {
        if (temp->isbn == cariISBN) {
            cout << "ISBN   : " << temp->isbn << endl;
            cout << "Judul  : " << temp->judul << endl;
            cout << "Penulis: " << temp->penulis << endl;
            return;
        }
        temp = temp->next;
    }
    cout << "Buku dengan ISBN '" << cariISBN << "' tidak ditemukan!\n";
}

int main() {
    Buku* head = NULL;
    int pilihan;
    string isbn, judul, penulis, cari;
    
    // Tambah data sample
    tambahBuku(&head, "111", "Pemrograman C++", "Budi Santoso");
    tambahBuku(&head, "222", "Struktur Data", "Ani Wijaya");
    tambahBuku(&head, "333", "Algoritma Dasar", "Budi Santoso");
    tambahBuku(&head, "444", "Database System", "Citra Dewi");
    
    cout << "SISTEM MANAJEMEN BUKU PERPUSTAKAAN\n";
    
    do {
        cout << "\n=== MENU UTAMA ===\n";
        cout << "1. Tambah Buku\n";
        cout << "2. Hapus Buku\n";
        cout << "3. Perbarui Buku\n";
        cout << "4. Lihat Semua Buku\n";
        cout << "5. Lihat Satu Buku\n";
        cout << "6. Cari Buku Berdasarkan Judul\n";
        cout << "7. Cari Buku Berdasarkan Penulis\n";
        cout << "8. Cari Buku Berdasarkan ISBN\n";
        cout << "9. Keluar\n";
        cout << "Pilihan: ";
        cin >> pilihan;
        cin.ignore();
        
        switch (pilihan) {
            case 1:
                cout << "ISBN: ";
                getline(cin, isbn);
                cout << "Judul: ";
                getline(cin, judul);
                cout << "Penulis: ";
                getline(cin, penulis);
                tambahBuku(&head, isbn, judul, penulis);
                break;
                
            case 2:
                cout << "Masukkan ISBN buku yang akan dihapus: ";
                getline(cin, isbn);
                hapusBuku(&head, isbn);
                break;
                
            case 3:
                cout << "Masukkan ISBN buku yang akan diperbarui: ";
                getline(cin, isbn);
                perbaruiBuku(head, isbn);
                break;
                
            case 4:
                lihatSemuaBuku(head);
                break;
                
            case 5:
                cout << "Masukkan ISBN buku yang akan dilihat: ";
                getline(cin, isbn);
                lihatSatuBuku(head, isbn);
                break;
                
            case 6:
                cout << "Masukkan judul buku yang dicari: ";
                getline(cin, cari);
                cariBukuJudul(head, cari);
                break;
                
            case 7:
                cout << "Masukkan nama penulis yang dicari: ";
                getline(cin, cari);
                cariBukuPenulis(head, cari);
                break;
                
            case 8:
                cout << "Masukkan ISBN yang dicari: ";
                getline(cin, cari);
                cariBukuISBN(head, cari);
                break;
                
            case 9:
                cout << "Program selesai. Terima kasih!\n";
                break;
                
            default:
                cout << "Pilihan tidak valid!\n";
        }
    } while (pilihan != 9);
    
    return 0;
}
```

> Output
> ![Screenshot bagian x](https://github.com/Nashiw/Laporan-Praktikum/blob/main/Modul%205/jawaban%202.png)

Program ini merupakan sistem manajemen buku perpustakaan yang menggunakan struktur data linked list untuk menyimpan data buku yang terdiri dari ISBN, judul, dan penulis, dengan fitur-fitur dasar seperti menambah, menghapus, memperbarui, dan melihat data buku, serta telah ditambahkan tiga fungsi pencarian (searching) yang memungkinkan pengguna untuk mencari buku berdasarkan judul dengan pencarian parsial yang dapat menemukan buku yang mengandung kata kunci tertentu dalam judulnya, berdasarkan penulis yang dapat menampilkan semua buku dari penulis yang dicari, dan berdasarkan ISBN dengan pencarian eksak yang hanya mengembalikan satu hasil karena sifat ISBN yang unik, di mana semua operasi pencarian dilakukan menggunakan linear search dengan menelusuri setiap node dalam linked list secara berurutan dari awal hingga akhir.


## Referensi

1. https://www.w3schools.com/cpp/cpp_for_loop_nested.asp
2. https://www.w3schools.com/cpp/cpp_arrays.asp
3. https://www.w3schools.com/cpp/cpp_arrays_loop.asp
4. https://www.w3schools.com/cpp/cpp_references.asp
5. https://www.w3schools.com/cpp/cpp_pointers.asp
6. https://www.w3schools.com/cpp/cpp_function_param.asp
7. https://www.w3schools.com/cpp/cpp_function_array.asp
8. https://www.geeksforgeeks.org/dsa/linked-list-data-structure/
9. https://en.wikipedia.org/wiki/Linked_list

