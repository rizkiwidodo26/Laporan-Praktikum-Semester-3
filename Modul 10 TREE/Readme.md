# <h1 align="center">Laporan Praktikum Modul 10 <br>TREE</h1>
<p align="center"> RIZKI WIDODO - 103112400136</p>

## Dasar Teori
Struktur data pohon merupakan representasi hierarkis non-linear yang terdiri dari kumpulan simpul (node) yang terhubung, diawali oleh simpul akar (root) tanpa induk, sementara simpul lain memiliki satu induk dan dapat memiliki beberapa anak, di mana simpul tanpa anak disebut daun. Jenis khususnya adalah Binary Tree yang membatasi setiap simpul maksimal memiliki dua anak, yaitu anak kiri dan kanan, dengan pengembangan lebih lanjut menjadi Binary Search Tree (BST) yang mengatur penempatan nilai berdasarkan aturan: seluruh nilai di subpohon kiri lebih kecil dari induknya dan nilai di subpohon kanan lebih besar, sehingga mempercepat proses pencarian, penyisipan, dan penghapusan data. Penelusuran data dilakukan melalui tiga metode traversal—pre-order (akar, kiri, kanan), in-order (kiri, akar, kanan) yang pada BST menghasilkan urutan menaik, dan post-order (kiri, kanan, akar)—yang umumnya diimplementasikan menggunakan fungsi rekursif, di mana fungsi memanggil dirinya sendiri untuk menangani setiap subpohon secara berulang, memungkinkan pengelolaan data yang efisien dan terstruktur dalam pemrograman.


## Guided

### Guided 1
```c++
#include <iostream>
using namespace std;

struct Node
{
    int data;
    Node *kiri, *kanan;
};

Node *buatNode(int nilai)
{
    Node *baru = new Node();
    baru->data = nilai;
    baru->kiri = baru->kanan = NULL;
    return baru;
}

Node *insert(Node *root, int nilai)
{
    if (root == NULL)
        return buatNode(nilai);
    
    if (nilai < root->data)
        root->kiri = insert(root->kiri, nilai);
    else if (nilai > root->data)
        root->kanan = insert(root->kanan, nilai);

    return root;
}

Node *search(Node *root, int nilai)
{
    if (root == NULL || root->data == nilai)
        return root;

    if (nilai < root->data)
        return search(root->kiri, nilai);

    return search(root->kanan, nilai);
}

Node *nilaiTerkecil(Node *node)
{
    Node *current = node;
    while (current && current->kiri != NULL)
        current = current->kiri;

        return current;
}

Node *hapus(Node *root, int nilai)
{
    if (root == NULL)
        return root;

    if (nilai < root->data)
        root->kiri = hapus(root->kiri, nilai);
    else if (nilai > root->data)
        root->kanan = hapus(root->kanan, nilai);
    else
    {
        if (root->kiri == NULL)
        {
            Node *temp = root->kanan;
            delete root;
            return temp;
        }
        else if (root->kanan == NULL){
            Node *temp = root->kiri;
            delete root;
            return temp;
        }
        Node *temp = nilaiTerkecil(root->kanan);
        root->data = temp->data;
        root->kanan = hapus(root->kanan, temp->data);
    }
    return root;
}

Node *update(Node *root, int Lama, int baru)
{
    if (search(root, Lama) != NULL)
    {
        root = hapus(root, Lama);
        root = insert(root, baru);
        cout << "Data " << Lama << " diupdate menjadi " << baru << endl;
    }
    else
    {
        cout << "Data " << Lama << " tidak ditemukan!" << endl;
    }
    return root;
}

void preOrder(Node *root)
{
    if (root != NULL)
    {
        cout << root->data << " ";
        preOrder(root->kiri);
        preOrder(root->kanan);
    }
}

void inOrder(Node *root)
{
    if (root != NULL)
    {
        inOrder(root->kiri);
        cout << root->data << " ";
        inOrder(root->kanan);
    }
}

void postOrder(Node *root)
{
    if (root != NULL)
    {
        postOrder(root->kiri);
        postOrder(root->kanan);
        cout << root->data << " ";
    }
}

int main()
{
    Node *root = NULL;

    cout << "=== 1. INSERT DATA ===" << endl;
    root = insert(root, 10);
    insert(root, 5);
    insert(root, 20);
    insert(root, 3);
    insert(root, 7);
    insert(root, 15);
    insert(root, 25);
    cout << "Data berhasil dimasukan.\n" << endl;

    cout << "=== 2. TAMPILKAN TREE (TRAVELSAL) ===" << endl;
    cout << "PreOrder : ";
    preOrder(root);
    cout << endl;
    cout << "InOrder : ";
    inOrder(root);
    cout << endl;
    cout << "PostOrder : ";
    postOrder(root);
    cout << "\n" << endl;

    cout << "=== 3. TEST SEARCH ===" << endl;
    int cari1 = 7, cari2 = 99;
    cout << "Cari " << cari1 << ": " << (search(root,cari1) ? "Ketemu" : "Tidak Aada") << endl;
    cout << "Cari " << cari2 << ": " << (search(root,cari2) ? "Ketemu" : "Tidak Aada") << endl;
    cout << endl;

    cout << "=== 4. TEST UPDATE ===" << endl;
    root = update(root, 5, 8);
    cout << "Hasil Order setelah update: ";
    cout << endl;
    cout << endl;

    cout << "PreOrder : ";
    preOrder(root);
    cout << endl;
    cout << "InOrder : ";
    inOrder(root);
    cout << endl;
    cout << "PostOrder : ";
    postOrder(root);
    cout << "\n" << endl;

    cout << "== 5. TEST DELETE ===" << endl;
    cout << "Menghapus angka 20..." << endl;
    root = hapus(root, 20);

    cout << "PreOrder : ";
    preOrder(root);
    cout << endl;
    cout << "InOrder : ";
    inOrder(root);
    cout << endl;
    cout << "PostOrder : ";
    postOrder(root);
    cout << "\n" << endl;

    return 0;
}
```

> Output
> 
> ![Screenshot bagian x](Output/Guided.png)

Program ini menyajikan realisasi struktur data Binary Search Tree (BST) secara utuh dalam bahasa C++, yang meliputi seluruh fungsi pengelolaan data (Create, Read, Update, Delete). Di samping kemampuan penyisipan data yang mempertahankan urutan pohon dan pencarian elemen, kode ini juga menyediakan fitur penghapusan node yang dapat menangani tiga kondisi berbeda: node tanpa anak, node dengan satu anak, serta node dengan dua anak melalui pengambilan penerus (successor) dari subtree kanan. Proses pembaruan nilai dilakukan dengan pendekatan yang menjaga integritas struktur, yaitu dengan menghapus nilai lama terlebihbih dahulu lalu memasukkan nilai baru, sehingga pohon tetap memenuhi sifat BST. Keberhasilan operasi-operasi tersebut dapat diperiksa melalui tiga teknik penelusuran yang disediakan, yaitu Pre-Order, In-Order, dan Post-Order.


## UNGUIDED

#### bstree.h
```c++
#ifndef BSTREE_H
#define BSTREE_H

struct Node {
    int nilai;
    Node* kiri;
    Node* kanan;
};

Node* buatNode(int x);
void tambahNode(Node* &akar, int x);
void tampilInOrder(Node* akar);
int hitungNode(Node* akar);
int hitungTotal(Node* akar);
int hitungKedalaman(Node* akar);
void tampilPreOrder(Node* akar);
void tampilPostOrder(Node* akar);

#endif
```
#### bstree.cpp
```c++
#include <iostream>
#include "bstree.h"
using namespace std;

Node* buatNode(int x) {
    Node* baru = new Node;
    baru->nilai = x;
    baru->kiri = NULL;
    baru->kanan = NULL;
    return baru;
}

void tambahNode(Node* &akar, int x) {
    if (akar == NULL) {
        akar = buatNode(x);
    } else if (x < akar->nilai) {
        tambahNode(akar->kiri, x);
    } else if (x > akar->nilai) {
        tambahNode(akar->kanan, x);
    }
}

void tampilInOrder(Node* akar) {
    if (akar != NULL) {
        tampilInOrder(akar->kiri);
        cout << akar->nilai << " - ";
        tampilInOrder(akar->kanan);
    }
}

int hitungNode(Node* akar) {
    if (akar == NULL) return 0;
    return 1 + hitungNode(akar->kiri) + hitungNode(akar->kanan);
}

int hitungTotal(Node* akar) {
    if (akar == NULL) return 0;
    return akar->nilai + hitungTotal(akar->kiri) + hitungTotal(akar->kanan);
}

int hitungKedalaman(Node* akar) {
    if (akar == NULL) return 0;
    int kiri = hitungKedalaman(akar->kiri);
    int kanan = hitungKedalaman(akar->kanan);
    if (kiri > kanan) return kiri + 1;
    return kanan + 1;
}

void tampilPreOrder(Node* akar) {
    if (akar != NULL) {
        cout << akar->nilai << " - ";
        tampilPreOrder(akar->kiri);
        tampilPreOrder(akar->kanan);
    }
}

void tampilPostOrder(Node* akar) {
    if (akar != NULL) {
        tampilPostOrder(akar->kiri);
        tampilPostOrder(akar->kanan);
        cout << akar->nilai << " - ";
    }
}
```
#### main.cpp
```c++
#include <iostream>
#include "bstree.h"
using namespace std;

int main() {
    Node* akar = NULL;
    
    // Masukkan data
    tambahNode(akar, 1);
    tambahNode(akar, 2);
    tambahNode(akar, 6);
    tambahNode(akar, 4);
    tambahNode(akar, 5);
    tambahNode(akar, 3);
    tambahNode(akar, 6);
    tambahNode(akar, 7);
    
    // Soal 1
    cout << "=== HASIL SOAL 1 ===" << endl;
    cout << "Hello World" << endl;
    tampilInOrder(akar);
    cout << endl << endl;
    
    // Soal 2
    cout << "=== HASIL SOAL 2 ===" << endl;
    cout << "kedalaman : " << hitungKedalaman(akar) << endl;
    cout << "jumlah node : " << hitungNode(akar) << endl;
    cout << "total : " << hitungTotal(akar) << endl;
    cout << endl;
    
    // Soal 3
    cout << "=== HASIL SOAL 3 ===" << endl;
    cout << "Pre-order: ";
    tampilPreOrder(akar);
    cout << endl;
    cout << "Post-order: ";
    tampilPostOrder(akar);
    cout << endl;
    
    return 0;
}
```
> Output soal 1,2,3
> 
> ![Screenshot bagian x](OUTPUT/unguided1.png)

Program ini mengimplementasikan struktur data Binary Search Tree (BST) menggunakan bahasa C++ dengan representasi linked list, di mana setiap node memiliki pointer ke anak kiri dan kanan . Program mencakup prosedur penyisipan data (insert) secara rekursif yang secara otomatis mengurutkan nilai—menempatkan angka yang lebih kecil di subtree kiri dan yang lebih besar di subtree kanan—serta menyediakan tiga metode penelusuran (traversal) yaitu In-Order, Pre-Order, dan Post-Order untuk menampilkan isi pohon . Selain operasi dasar, program ini juga dilengkapi dengan fungsi rekursif untuk melakukan perhitungan statistik pohon, meliputi penghitungan kedalaman maksimum (height/depth), jumlah total node, dan penjumlahan seluruh nilai data, yang kemudian hasilnya ditampilkan langsung ke layar sesuai dengan urutan data uji pada modul .


## Referensi

1. https://www.geeksforgeeks.org/cpp/cpp-binary-search-tree/ (diakses pada 29 November 2025)
2. https://www.tutorialspoint.com/data_structures_algorithms/tree_data_structure.htm (diakses pada 29 November 2025)
3. https://www.programiz.com/dsa/binary-search-tree (diakses pada 29 November 2025)
