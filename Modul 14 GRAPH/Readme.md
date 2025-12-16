# <h1 align="center">Laporan Praktikum Modul 14 <br>GRAPH</h1>
<p align="center">RIZKI WIDODO - 103112400136</p>

## Dasar Teori
Graph adalah struktur data non-linear yang merepresentasikan himpunan objek (disebut vertex atau node) yang terhubung oleh sisi (edge) yang dapat memiliki arah (pada directed graph) maupun tidak (pada undirected graph). Dalam implementasinya, graph dapat direpresentasikan menggunakan adjacency matrix atau adjacency list (multilist), dengan adjacency list sering dipilih karena efisiensi memorinya untuk graph yang sparse. Algoritma dasar pada graph meliputi penelusuran seperti Breadth-First Search (BFS) yang menjelajahi graph per level dan Depth-First Search (DFS) yang menjelajahi secara mendalam, serta topological sort untuk mengurutkan vertex pada directed acyclic graph berdasarkan dependensi antar node.

## Guided

### Guided 1
#### graf.h
```c++
#ifndef GRAF_H_INCLUDED
#define GRAF_H_INCLUDED

#include <iostream>
using namespace std;

typedef char infoGraph;

struct ElmNode;
struct ElmEdge;

typedef ElmNode *adrNode;
typedef ElmEdge *adrEdge;

struct ElmNode
{
    infoGraph info;
    int visited;
    adrEdge firstEdge;
    adrNode next;
};

struct ElmEdge
{
    adrNode node;
    adrEdge next;
};

struct Graph
{
    adrNode first;
};

// PRIMITIF GRAPH
void CreateGraph(Graph &G);
adrNode AllocateNode(infoGraph X);
adrEdge AllocateEdge(adrNode N);

void InsertNode(Graph &G, infoGraph X);
adrNode FindNode(Graph G, infoGraph X);

void ConnectNode(Graph &G, infoGraph A, infoGraph B);

void PrintInfoGraph(Graph G);

// Traversal
void ResetVisited(Graph &G);
void PrintDFS(Graph &G, adrNode N);
void PrintBFS(Graph &G, adrNode N);

#endif
```
#### graf.cpp
```c++
#include "graf.h"
#include <queue>
#include <stack>

void CreateGraph(Graph &G)
{
    G.first = NULL;
}

adrNode AllocateNode(infoGraph X)
{
    adrNode P = new ElmNode;
    P->info = X;
    P->visited = 0;
    P->firstEdge = NULL;
    P->next = NULL;
    return P;
}

adrEdge AllocateEdge(adrNode N)
{
    adrEdge P = new ElmEdge;
    P->node = N;
    P->next = NULL;
    return P;
}

void InsertNode(Graph &G, infoGraph X)
{
    adrNode P = AllocateNode(X);
    P->next = G.first;
    G.first = P;
}

adrNode FindNode(Graph G, infoGraph X)
{
    adrNode P = G.first;
    while (P != NULL)
    {
        if (P->info == X)
            return P;
        P = P->next;
    }
    return NULL;
}

void ConnectNode(Graph &G, infoGraph A, infoGraph B)
{
    adrNode N1 = FindNode(G, A);
    adrNode N2 = FindNode(G, B);

    if (N1 == NULL || N2 == NULL)
    {
        cout << "Node tidak ditemukan!\n";
        return;
    }

    // Buat edge dari N1 ke N2
    adrEdge E1 = AllocateEdge(N2);
    E1->next = N1->firstEdge;
    N1->firstEdge = E1;

    // Karena undirected → buat edge balik
    adrEdge E2 = AllocateEdge(N1);
    E2->next = N2->firstEdge;
    N2->firstEdge = E2;
}

void PrintInfoGraph(Graph G)
{
    adrNode P = G.first;
    while (P != NULL)
    {
        cout << P->info << " -> ";
        adrEdge E = P->firstEdge;
        while (E != NULL)
        {
            cout << E->node->info << " ";
            E = E->next;
        }
        cout << endl;
        P = P->next;
    }
}

void ResetVisited(Graph &G)
{
    adrNode P = G.first;
    while (P != NULL)
    {
        P->visited = 0;
        P = P->next;
    }
}

void PrintDFS(Graph &G, adrNode N)
{
    if (N == NULL)
        return;

    N->visited = 1;
    cout << N->info << " ";

    adrEdge E = N->firstEdge;
    while (E != NULL)
    {
        if (E->node->visited == 0)
        {
            PrintDFS(G, E->node);
        }
        E = E->next;
    }
}

void PrintBFS(Graph &G, adrNode N)
{
    if (N == NULL)
        return;

    queue<adrNode> Q;
    Q.push(N);

    while (!Q.empty())
    {
        adrNode curr = Q.front();
        Q.pop();

        if (curr->visited == 0)
        {
            curr->visited = 1;
            cout << curr->info << " ";

            adrEdge E = curr->firstEdge;
            while (E != NULL)
            {
                if (E->node->visited == 0)
                {
                    Q.push(E->node);
                }
                E = E->next;
            }
        }
    }
}

```
#### main.cpp
```c++
#include "graf.h"
#include "graf.cpp"
#include <iostream>
using namespace std;

int main()
{
    Graph G;
    CreateGraph(G);

    // Tambah node
    InsertNode(G, 'A');
    InsertNode(G, 'B');
    InsertNode(G, 'C');
    InsertNode(G, 'D');
    InsertNode(G, 'E');

    // Hubungkan node (graph tidak berarah)
    ConnectNode(G, 'A', 'B');
    ConnectNode(G, 'A', 'C');
    ConnectNode(G, 'B', 'D');
    ConnectNode(G, 'C', 'E');

    cout << "=== Struktur Graph ===\n";
    PrintInfoGraph(G);

    cout << "\n=== DFS dari Node A ===\n";
    ResetVisited(G);
    PrintDFS(G, FindNode(G, 'A'));

    cout << "\n\n=== BFS dari Node A ===\n";
    ResetVisited(G);
    PrintBFS(G, FindNode(G, 'A'));

    cout << endl;
    return 0;
}

```

> Output
> 
> ![Screenshot bagian x](Output/Guided1.png)

Program ini mengimplementasikan struktur data graf menggunakan representasi adjacency list. Setiap simpul dialokasikan dalam memori sebagai elemen terpisah dan dihubungkan melalui linked list utama. Fungsi penghubung simpul dirancang untuk graf tidak berarah, di mana setiap koneksi antara dua simpul dibuat secara timbal balik dengan menambahkan referensi ke daftar tetangga kedua simpul.

## UNGUIDED 1,2,3


#### code
#### graph.h
```c++
#ifndef GRAF_H
#define GRAF_H

typedef char infoGraph;
typedef struct ElmNode* adrNode;
typedef struct ElmEdge* adrEdge;

struct ElmNode {
    infoGraph info;
    int visited;
    adrEdge firstEdge;
    adrNode next;
};

struct ElmEdge {
    adrNode node;
    adrEdge next;
};

struct Graph {
    adrNode first;
};

void CreateGraph(Graph* G);
void InsertNode(Graph* G, infoGraph X);
void ConnectNode(Graph* G, infoGraph A, infoGraph B);
void PrintInfoGraph(Graph G);
void PrintDFS(Graph G, adrNode N);
void PrintBFS(Graph G, adrNode N);
adrNode CariNode(Graph G, infoGraph X);

#endif
```
#### graph.cpp
```c++
#include <stdio.h>
#include <stdlib.h>
#include "graf.h"

void CreateGraph(Graph* G) { G->first = NULL; }

adrNode BuatNode(infoGraph X) {
    adrNode baru = (adrNode)malloc(sizeof(struct ElmNode));
    baru->info = X; baru->visited = 0;
    baru->firstEdge = NULL; baru->next = NULL;
    return baru;
}

adrEdge BuatEdge(adrNode tujuan) {
    adrEdge baru = (adrEdge)malloc(sizeof(struct ElmEdge));
    baru->node = tujuan; baru->next = NULL;
    return baru;
}

void InsertNode(Graph* G, infoGraph X) {
    adrNode baru = BuatNode(X);
    if (G->first == NULL) G->first = baru;
    else {
        adrNode akhir = G->first;
        while (akhir->next != NULL) akhir = akhir->next;
        akhir->next = baru;
    }
}

adrNode CariNode(Graph G, infoGraph X) {
    adrNode cari = G.first;
    while (cari != NULL) {
        if (cari->info == X) return cari;
        cari = cari->next;
    }
    return NULL;
}

void ConnectNode(Graph* G, infoGraph A, infoGraph B) {
    adrNode nodeA = CariNode(*G, A);
    adrNode nodeB = CariNode(*G, B);
    if (nodeA == NULL || nodeB == NULL) return;
    
    adrEdge edge1 = BuatEdge(nodeB);
    edge1->next = nodeA->firstEdge;
    nodeA->firstEdge = edge1;
    
    adrEdge edge2 = BuatEdge(nodeA);
    edge2->next = nodeB->firstEdge;
    nodeB->firstEdge = edge2;
}

void PrintInfoGraph(Graph G) {
    char urutan[] = {'A','B','C','D','E','F'};
    for (int i = 0; i < 6; i++) {
        adrNode node = CariNode(G, urutan[i]);
        if (node != NULL) {
            printf("[%c] terhubung ke -> ", node->info);
            adrEdge edge = node->firstEdge;
            while (edge != NULL) {
                printf("%c", edge->node->info);
                edge = edge->next;
                if (edge != NULL) printf(" ");
            }
            printf("\n");
        }
    }
}

void resetStatus(Graph* G) {
    adrNode node = G->first;
    while (node != NULL) {
        node->visited = 0;
        node = node->next;
    }
}

void DFS(adrNode node) {
    if (node == NULL || node->visited == 1) return;
    node->visited = 1;
    printf("%c ", node->info);
    adrEdge edge = node->firstEdge;
    while (edge != NULL) {
        if (edge->node->visited == 0) DFS(edge->node);
        edge = edge->next;
    }
}

void PrintDFS(Graph G, adrNode N) {
    resetStatus(&G);
    printf("DFS Traversal: ");
    DFS(N);
    printf("\n");
}

void PrintBFS(Graph G, adrNode N) {
    if (N == NULL) return;
    resetStatus(&G);
    printf("BFS Traversal: ");
    
    adrNode queue[100];
    int depan = 0, belakang = 0;
    N->visited = 1;
    queue[belakang++] = N;
    
    while (depan < belakang) {
        adrNode sekarang = queue[depan++];
        printf("%c ", sekarang->info);
        adrEdge edge = sekarang->firstEdge;
        while (edge != NULL) {
            if (edge->node->visited == 0) {
                edge->node->visited = 1;
                queue[belakang++] = edge->node;
            }
            edge = edge->next;
        }
    }
    printf("\n");
}
```
#### main.cpp
```c++
#include "graf.h"
#include <stdio.h>

int main() {
    Graph G;
    CreateGraph(&G);
    
    InsertNode(&G, 'A'); InsertNode(&G, 'B'); InsertNode(&G, 'C');
    InsertNode(&G, 'D'); InsertNode(&G, 'E'); InsertNode(&G, 'F');
    
    ConnectNode(&G, 'A', 'B'); ConnectNode(&G, 'A', 'D');
    ConnectNode(&G, 'B', 'C'); ConnectNode(&G, 'B', 'E');
    ConnectNode(&G, 'C', 'F'); ConnectNode(&G, 'D', 'E');
    ConnectNode(&G, 'E', 'F');
    
    printf("=== Graph Adjacency List ===\n");
    PrintInfoGraph(G);
    printf("\n");
    
    adrNode nodeA = CariNode(G, 'A');
    PrintDFS(G, nodeA);
    PrintBFS(G, nodeA);
    
    return 0;
}
```
> Output soal 1
> 
> ![Screenshot bagian x](Output/unguided1.png)

Program yang merupakan representasi Adjacency List berbasis pointer dalam bahasa C++. Program terbagi menjadi tiga file: graph.h sebagai header yang mendefinisikan struktur data verteks dan edge, graph.cpp yang berisi logika manipulasi graph, dan main.cpp sebagai driver untuk pengujian. Fitur utama program ini meliputi pembuatan graph, penambahan verteks, serta penghubungan antar verteks (edge) yang bersifat dua arah. Selain itu, program ini mengimplementasikan dua algoritma penelusuran graph, yaitu DFS (Depth First Search) yang bekerja secara rekursif, dan BFS (Breadth First Search) yang menggunakan struktur data Queue manual (dibuat sendiri tanpa library STL <queue>) untuk menelusuri node secara melebar.


## Referensi

1. https://algomap.io/lessons/graphs (diakses pada 15 Desember 2025)
2. https://terapan-ti.vokasi.unesa.ac.id/post/teori-graf-pengertian-jenis-representasi-dan-algoritma (diakses pada 15 Desember 2025)
3. https://www.geeksforgeeks.org/dsa/breadth-first-search-or-bfs-for-a-graph/ (diakses pada 15 Desember 2025)
