# TEKNIK PENCARIAN BLIND SEARCH

## Tujuan Praktikum 
Mahasiswa mampu memahami konsep blind search dan dapat mengimplementasikan program salah satu algoritma blind search pada kasus tree. Program ini dibuat dengan menggunakan bahasa pemrograman Java.

## Dasar Teori
Teknik Pencarian Blind Search
Ada beberapa algoritma yang dikategorikan ke dalam teknik pencarian blind search.
1. Breadth First Search (BFS)
2. Uniform Cost Search (UCS)
3. Depth First Search (DFS)
4. Depth Limited Search (DLS)
5. Iterative Deepening Search (IDS)
6. Bidirectional Search (BS)

Pada praktikum kali ini kita menggunakan alogaritma Breadth First Search (BFS) adalah algoritma yang menjelajah node root pertama sekali, kemudian menjelajah semua successor dari node root, kemudian menjelajah semua successor dari successor, dan seterusnya sampai successor yang terakhir. Fringe merupakan struktur data queue First In First Out (FIFO).

Kode berikut merupakan urutan angka dari 0, 1, 2, N. Initial state-nya adalah 0 dan goal state-nya adalah angka yang ditetapkan oleh user. Setiap langkah dibangkitkan secara acak atau 1. Method addEdge(Node n1, Node n2) adalah method untuk menghubungkan dua buah edge sedangkan method bfs(Node s) adalah method untuk menentukan teknik pencarian menggunakan algoritma Breadth First Search.
# Code

    import java.util.ArrayDeque;
    import java.util.ArrayList;
    import java.util.HashMap;
    import java.util.List;
    import java.util.Map;
    import java.util.Queue;
    import java.util.Set;

    public class BlindSearch {
        public enum NodeColour {
            WHITE, GRAY, BLACK
    }

    public static class Node {
        int data;
        int distance;
        Node predecessor;
        NodeColour colour;

    public Node(int data)

    {
          this.data = data;
    }

        public String toString() {
            return "(" + data + ",d=" + distance + ")";
        }
    }

    Map<Node, List<Node>> nodes;

    public BlindSearch()
    {
    nodes = new HashMap<Node, List<Node>>();
    }

    public void addEdge(Node n1, Node n2) {
        if (nodes.containsKey(n1)) {
            nodes.get(n1).add(n2);
        } else {
            ArrayList<Node> list = new ArrayList<Node>();
            list.add(n2);
            nodes.put(n1, list);
        }
    }

    public void bfs(Node s) {
        Set<Node> keys = nodes.keySet();
        for (Node u : keys) {
            if (u != s) {
                u.colour = NodeColour.WHITE;
                u.distance = Integer.MAX_VALUE;
                u.predecessor = null;
            }
        }
        s.colour = NodeColour.GRAY;
        s.distance = 0;
        s.predecessor = null;
        Queue<Node> q = new ArrayDeque<Node>();
        q.add(s);
        while (!q.isEmpty()) {
            Node u = q.remove();
            List<Node> adj_u = nodes.get(u);
            if (adj_u != null) {
                for (Node v : adj_u) {
                    if (v.colour == NodeColour.WHITE) {
                        v.colour = NodeColour.GRAY;
                        v.distance = u.distance + 1;
                        v.predecessor = u;
                        q.add(v);
                    }
                }
            }
            u.colour = NodeColour.BLACK;
            System.out.print(u + " ");
        }
    }

    public static void main(String[] args)

    {
        BlindSearch graph = new BlindSearch();
        Node n1 = new Node(1);
        Node n2 = new Node(2);
        Node n3 = new Node(3);
        Node n4 = new Node(4);
        Node n5 = new Node(5);
        Node n6 = new Node(6);
        Node n7 = new Node(7);
        Node n8 = new Node(8);
        
        graph.addEdge(n1, n2);
        
        graph.addEdge(n2, n1);
        graph.addEdge(n2, n3);
        
        graph.addEdge(n3, n4);
        graph.addEdge(n3, n2);
        
        graph.addEdge(n4, n3);
        graph.addEdge(n4, n5);
        graph.addEdge(n4, n6);
        
        graph.addEdge(n5, n4);
        graph.addEdge(n5, n6);
        graph.addEdge(n5, n7);
        
        graph.addEdge(n6, n4);
        graph.addEdge(n6, n5);
        graph.addEdge(n6, n7);
        graph.addEdge(n6, n8);
        
        graph.addEdge(n7, n5);
        graph.addEdge(n7, n6);
        graph.addEdge(n7, n8);
        
        graph.addEdge(n8, n6);
        graph.addEdge(n8, n7);
        
        graph.bfs(n3);
      }
    }

Untuk kode diatas memiliki output (3,d=0) (4,d=1) (2,d=1) (5,d=2) (6,d=2) (1,d=2) (7,d=3) (8,d=3) 

Gambar 4.5

![image](https://github.com/kausaraghnia/kausaraghnia/assets/148691014/88b90030-4ca8-4c30-b3e5-2f3e6187e3b1)

Gambar 4.6

![image](https://github.com/kausaraghnia/kausaraghnia/assets/148691014/0bfdbf6e-a3d7-42b4-a1d3-bad657045aab)

Gmabar 4.7

![image](https://github.com/kausaraghnia/kausaraghnia/assets/148691014/4ed2510b-8471-4b68-be1a-652ac7916eeb)

# Tugas
1. Tentukan bagaimana algoritma BFS di atas dapat menentukan node ke 8, 6, dan 7.
2. Ubahlah method static void main sehingga bentuk tree seperti Gambar 4.5 dapat dibentuk. Kemudian tentukan bagaimana algoritma BFS dapat menemukan node 5.
3. Ubahlah method static void main sehingga bentuk tree seperti Gambar 4.6 dapat dibentuk. Kemudian tentukan bagaimana algoritma BFS dapat menemukan node 9.
4. Ubahlah kode program di atas sehingga bentuk tree seperti Gambar 4.7 dapat dibentuk. Kemudian tentukan bagaimana algoritma BFS dapat menemukan node C.

# Jawaban
# 1. Langkah langkah BFS di atas dapat menentukan node ke 8, 6, dan 7.

1. Mulai dari node awal, yaitu node 3 (n3), yang menjadi node awal yang di-pass ke metode bfs(n3).
2. Di awal algoritma, semua node dalam graph diinisialisasi dengan warna putih (WHITE) kecuali node awal (n3), yang diwarnai dengan abu-abu (GRAY) karena sedang diproses.
3.  Node awal (n3) memiliki jarak (distance) 0.
4.  Node-node yang terhubung langsung dengan node awal (n3) adalah node 2 (n2) dan node 4 (n4). Kedua node ini ditambahkan ke antrian (queue) untuk diproses selanjutnya.
5.  Algoritma BFS akan melakukan iterasi selama antrian tidak kosong.
6.  Node yang sedang diproses adalah node 3 (n3), yang memiliki adjacency list berisi node 2 (n2) dan node 4 (n4).
7.  Node 2 (n2) dan node 4 (n4) adalah node-node putih (WHITE) karena belum diproses. Algoritma BFS akan mengubah warna node 2 (n2) dan node 4 (n4) menjadi abu-abu (GRAY), menghitung jaraknya, dan mengatur node 3 (n3) sebagai pendahulunya.
    - Node 2 (n2) akan memiliki jarak 1 dari node awal (n3).
    - Node 4 (n4) akan memiliki jarak 1 dari node awal (n3).
8.  Node-node 2 (n2) dan 4 (n4) ditambahkan ke antrian (queue) untuk diproses pada langkah selanjutnya.
9.  Algoritma akan terus berlanjut dengan mengambil node-node dari antrian yang berikutnya untuk diproses.
10.  Algoritma akan terus melakukan langkah-langkah serupa hingga seluruh node terhubung dalam graph telah diproses.
11.  Node-node yang sudah selesai diproses akan diwarnai dengan hitam (BLACK).
12.  Hasil dari algoritma BFS adalah pencarian jarak terpendek dari node awal (n3) ke semua node lain dalam graph dan pengaturan node pendahulu untuk setiap node. Dengan demikian, Anda dapat menggunakan informasi ini untuk menentukan jarak dari node awal (n3) ke node 8 (n8), node 6 (n6), dan node 7 (n7).

Node 8 (n8) terhubung dengan node 6 (n6), yang terhubung dengan node 4 (n4), yang terhubung dengan node 3 (n3). Jarak dari n3 ke n4 adalah 1, jarak dari n4 ke n6 adalah 1, dan jarak dari n6 ke n8 adalah 1. Jadi, jarak dari n3 ke n8 adalah 3. Node 7 (n7) juga terhubung dengan node 6 (n6), yang terhubung dengan node 4 (n4), yang terhubung dengan node 3 (n3). Jarak dari n3 ke n4 adalah 1, jarak dari n4 ke n6 adalah 1, dan jarak dari n6 ke n7 adalah 1. Jadi, jarak dari n3 ke n7 adalah 3.

# 2.  Cara algoritma BFS dapat menemukan node 5.
Yang pertama kita ubah pada bagian static void nya menjadi seperti berikut

    public static void main(String[] args)

    {
        BS graph = new BS();
        Node n0 = new Node(0);
        Node n1 = new Node(1);
        Node n2 = new Node(2);
        Node n3 = new Node(3);
        Node n4 = new Node(4);
        Node n5 = new Node(5);
        Node n6 = new Node(6);
        
        
        
        // Ganti graph dan edge sesuai dengan yang diinginkan
        graph.addEdge(n0, n1);
        graph.addEdge(n0, n2);
       
        graph.addEdge(n1, n0);
        graph.addEdge(n1, n3);
        graph.addEdge(n1, n4);

        graph.addEdge(n2, n0);
        graph.addEdge(n2, n5);
        graph.addEdge(n2, n6);
       
        graph.addEdge(n3, n1);
        graph.addEdge(n3, n4);

        graph.addEdge(n4, n3);
        graph.addEdge(n4, n1);

        graph.addEdge(n5, n2);
        graph.addEdge(n5, n6);
        graph.addEdge(n5, n1);

        graph.addEdge(n6, n2);
        graph.addEdge(n6, n5);
        graph.addEdge(n6, n1);
        
        graph.bfs(n0);
    }

kode diatas memberikan output (0,d=0) (1,d=1) (2,d=1) (3,d=2) (4,d=2) (5,d=2) (6,d=2) 

Berikut langkah langkah BFS menemukan node 5 :
1. masukkan node 0 ke dalam antrian
2. periksa apakah node 5 ada di antrian. Tidak Ada.
3. Keluarkan node 0 dari antrian.
4. Periksa semua node yang bertetangga dengan node 0. Node 1 dan 2 tidak dikunjungi.
5. Masukkan node 2 dan 3 ke dalam antrian.
6. Periksa apakah node 5 ada di antrian. Tidak ada.
7. Keluarkan node 1 dari antrian.
8. Periksa semua node yang bertetangga dengan node 1. Node 3 dan 4 tidak dikunjungi.
9. Masukkan node 3 dan 4 ke dalam antrian.
10. Periksa apakah node 5 ada di antrian. Tidak ada.
11. Kembali ke node yang bertetanggaan dengan node 0 yaitu node 1 dan 2. node 1 sudah diperiksa.
12. Periksa semua node yang bertetangga dengan node 2. Node 5 dan 6 tidak dikunjungi.
13. Masukkan node 5 dan 6 ke dalam antrian.
14. Periksa apakah node 5 ada di antrian. Ya ada, node 5 ditemukan

# 3. Cara alogaritma BFS dapat menemukan node 9
Yang pertama kita ubah static void nya menjadi seperti berikut

    public static void main(String[] args)

    {
        BS2 graph = new BS2();
        Node n1 = new Node(1);
        Node n2 = new Node(2);
        Node n3 = new Node(3);
        Node n4 = new Node(4);
        Node n5 = new Node(5);
        Node n6 = new Node(6);
        Node n7 = new Node(7);
        Node n8 = new Node(8);
        Node n9 = new Node(9);
        Node n10 = new Node(10);
        Node n11 = new Node(11);
        Node n12 = new Node(12);

        graph.addEdge(n1, n2);
        graph.addEdge(n1, n3);
        graph.addEdge(n1, n4);

        graph.addEdge(n2, n1);
        graph.addEdge(n2, n5);
        graph.addEdge(n2, n6);
        
        graph.addEdge(n3, n1);
        
        graph.addEdge(n4, n1);
        graph.addEdge(n4, n7);
        graph.addEdge(n4, n8);
        
        graph.addEdge(n5, n2);
        graph.addEdge(n5, n9);
        graph.addEdge(n5, n10);
        graph.addEdge(n5, n6);
        
        graph.addEdge(n6, n2);
        graph.addEdge(n6, n5);
        
        
        graph.addEdge(n7, n4);
        graph.addEdge(n7, n11);
        graph.addEdge(n7, n12);
        graph.addEdge(n7, n8);
        
        graph.addEdge(n8, n4);
        graph.addEdge(n8, n7);

        graph.addEdge(n9, n5);
        graph.addEdge(n9, n10);

        graph.addEdge(n10, n5);
        graph.addEdge(n10, n9);

        graph.addEdge(n11, n7);
        graph.addEdge(n11, n12);

        graph.addEdge(n12, n7);
        graph.addEdge(n12, n11);
        
        graph.bfs(n1);
    }

kode diatas memberikan output (1,d=0) (2,d=1) (3,d=1) (4,d=1) (5,d=2) (6,d=2) (7,d=2) (8,d=2) (9,d=3) (10,d=3) (11,d=3) (12,d=3)

1. Masukkan node 1 ke dalam antrian.
2. Periksa apakah node 9 ada di antrian. Tidak ada.
3. Keluarkan node 1 dari antrian.
4. Periksa semua node yang bertetangga dengan node 1. Node 2, 3 dan 4 tidak dikunjungi.
5. Masukkan node 2, 3 dan 4 ke dalam antrian.
6. Periksa apakah node 9 ada di antrian. Tidak ada.
7. Keluarkan node 2 dari antrian.
8. Periksa semua node yang bertetangga dengan node 2. Node 5 dan 6 tidak dikunjungi.
9. Masukkan node 5 dan 6 ke dalam antrian.
10. Periksa apakah node 9 ada di antrian. Tidak ada.
11. Keluarkan node 5 dari antrian.
12. Periksa semua node yang bertetangga dengan node 5. Node 9 dan 10 tidak dikunjungi.
13. Masukkan node 9 dan 10 kedalam antrian.
14. Periksa apakah node 9 ada di antrian. Ya ada, node 9 ditemukan

# 4. Cara alogaritma BFS menemukan Node C
Yang pertama ubah static void nya menjadi seperti dibawah

    public static void main(String[] args)

    {
        BS3 graph = new BS3();
        Node n1 = new Node('A');
        Node n2 = new Node('B');
        Node n3 = new Node('C');
        Node n4 = new Node('D');
        Node n5 = new Node('E');
        Node n6 = new Node('F');
        Node n7 = new Node('G');
        Node n8 = new Node('H');
        Node n9 = new Node('I');
        
        graph.addEdge(n1, n2);
        graph.addEdge(n1, n4);

        graph.addEdge(n2, n6);
        graph.addEdge(n2, n1);
        graph.addEdge(n2, n4);
        
        graph.addEdge(n3, n4);
        graph.addEdge(n3, n5);
        
        graph.addEdge(n4, n2);
        graph.addEdge(n4, n3);
        graph.addEdge(n4, n5);
        
        
        graph.addEdge(n5, n4);
        graph.addEdge(n5, n3);
        
        
        graph.addEdge(n6, n2);
        graph.addEdge(n6, n7);
       
        
        
        graph.addEdge(n7, n6);
        graph.addEdge(n7, n9);
        
        graph.addEdge(n8, n9);
        
        graph.addEdge(n9, n8);
        
        graph.bfs(n6);
    }

  Dan juga pada bagian yang bertuliskan int data yaitu pada public class dan public Node diganti dengan char karena kita menggunakan huruf bukan menggunakan angka

    public class BS3 {
      public enum NodeColour {
          WHITE, GRAY, BLACK
      }

      public static class Node {
          char data;
          int distance;
          Node predecessor;
          NodeColour colour;

      public Node(char data)

      {
        this.data = data;
      }

        public String toString() {
            return "(" + data + ",d=" + distance + ")";
        }
      }

kode diatas memberikan output (F,d=0) (B,d=1) (G,d=1) (A,d=2) (D,d=2) (I,d=2) (C,d=3) (E,d=3) (H,d=3) 

1. Masukkan node F ke dalam antrian.
2. Periksa apakah node C ada di antrian. Tidak ada.
3. Keluarkan node F dari antrian.
4. Periksa semua node yang bertetangga dengan node F. Node B dan G tidak dikunjungi.
5. Masukkan node B dan G ke dalam antrian.
6. Periksa apakah node C ada di antrian. Tidak ada.
7. Keluarkan node B dari antrian.
8. Periksa semua node yang bertetangga dengan node B. Node A dan D tidak dikunjungi.
9. Masukkan node A dan D ke dalam antrian.
10. Periksa apakah node C ada di antrian. Tidak ada.
11. Keluarkan node A dari antrian.
12. Periksa semua node yang bertetangga dengan node A. tidak ada
13. Keluarkan node D dari antrian.
14. Periksa semua node yang bertetangga dengan node D. Node C dan E tidak dikunjungi.
15. Masukkan node C dan E kedalam antrian.
16. Periksa apakah node C ada di antrian. Ya ada, node C ditemukan

  
