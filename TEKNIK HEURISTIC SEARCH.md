# Teknik Heuristic Search

## Tujuan Praktikum
Memperlihatkan kepada mahasiswa bagaimana menyelesaikan permasalahan pada game 8-puzzle  dengan menggunakan algoritma heuristic search. Mahasiswa diharapkan mampu  mengimplementasikan algoritma heuristic dengan menggunakan Java.

## Dasar teori

Teknik Pencarian Heuristic Search. 
Teknik blind search tidak selalu memecahkan masalah dengan baik. Waktu yang dibutuhkan  ketika menemukan solusi atau memecahkan masalah terlalu lama dan juga memori yang dibutuhkan untuk menampung urutan-urutan solusi sangat besar akan menjadi kelemahan bagi algoritma ini. Kelemahan tersebut dapat diatasi ketika informasi-informasi tambahan yang diperoleh dari setiap langkah pencarian diidentifikasikan dan dijadikan sebagai penentu langkah berikutnya. Salah satu teknik untuk meminimalisasikan kelemahan dari blind search adalah teknik  heuristic. Heuristic merupakan suatu proses dimana pencarian solusi akan ditemukan dengan baik namun bisa juga kemungkinan tidak ada solusi. Teknik ini mmebutuhkan sebuah nilai untuk menentukan pencarian berikutnya. Nilai heuristic dapat ditentukan melalui fungsi heuristic. Fungsi heuristic merupakan fungsi yang melakukan pemetaan dari diskripsi keadaan ke pengukur kebutuhan. Umumnya fungsi ini direpresentasikan ke dalam bentuk angka. Dalam ilmu Kecerdasan Buatan, heuristic dihadapkan dalam 2 keadaan dasar.
- Persoalan/problema yang mungkin memiliki solusi eksak, namun biaya perhitungan untuk menemukan solusi tersebut sangat tinggi dalam kebanyakan persoalan (seperti catur), ruang keadaan bertambah secara luar biasa seiring dengan jumlah.
- Persoalan yang mungkin tidak memiliki solusi eksak karena ambiquitas (ketidakpastian) mendasar dalam pernyataan persoalan atau data yang tersedia diagnosa medis merupakan salahmsatu contohnya.
Heuristi hanyalah sebuah cara menerka langkah berikutnya yang harus diambil dalam memecahkan suatu persoalan berdasarkan informasi yang ada/tersedia.

## Code

    import java.util.Vector;

    class Node {
    int[] state = new int[9];
    int cost;
    Node parent = null;
    Vector<Node> successors = new Vector<Node>();

    Node(int s[], Node parent) {
        this.parent = parent;
        for (int i = 0; i < 9; i++)
            state[i] = s[i];
    }

    public Node() {
    }

    public String toString() {
        String s = "";
        for (int i = 0; i < 9; i++) {
            s = s + state[i] + " ";
        }
        return s;
    }

    public boolean equals(Object node) {
        Node n = (Node) node;
        boolean result = true;
        for (int i = 0; i < 9; i++) {
            if (n.state[i] != state[i])
                result = false;
        }
        return result;
    }

    Vector<Node> getPath(Vector<Node> v) {
        v.insertElementAt(this, 0);
        if (parent != null)
            v = parent.getPath(v);
        return v;
    }

    Vector<Node> getPath() {
        return getPath(new Vector<Node>());
    }
    public static void main(String args[]) {
        new Node().run();
    }

    private void run() {
    }
    }

    class EightPuzzleSpace {
    Node getRoot() {
        int initialState[] ={3, 1, 2, 4, 7, 5, 6, 8, 0};
        return new Node(initialState, null);
    }

    Node getGoal() {
        int goalState[] = {1, 2, 3, 4, 0, 8, 5, 6, 7};
        return new Node(goalState, null);
    }

    Vector<Node> getSuccessors(Node parent) {
        Vector<Node> successors = new Vector<Node>();
        for (int r = 0; r < 3; r++) {
            for (int c = 0; c < 3; c++) {
                if (parent.state[(r * 3) + c] == 0) {
                    if (r > 0) {
                        successors.add(transformState(r - 1, c, r, c, parent));
                    }
                    if (r < 2) {
                        successors.add(transformState(r + 1, c, r, c, parent));
                    }
                    if (c > 0) {
                        successors.add(transformState(r, c - 1, r, c, parent));
                    }
                    if (c < 2) {
                        successors.add(transformState(r, c + 1, r, c, parent));
                    }
                }
            }
        }
        parent.successors = successors;
        return successors;
    }

    Node transformState(int r0, int c0, int r1, int c1, Node parent) {
        int[] s = parent.state;
        int[] newState = { s[0], s[1], s[2], s[3], s[4], s[5], s[6], s[7], s[8] };
        newState[(r1 * 3) + c1] = s[(r0 * 3) + c0];
        newState[(r0 * 3) + c0] = 0;
        return new Node(newState, parent);
    }
    
    public static void main(String args[]) {
        new EightPuzzleSpace().run();
    }

    private void run() {
    }


    
    
    }

    class EightPuzzleSearch {
    EightPuzzleSpace space = new EightPuzzleSpace();
    Vector<Node> open = new Vector<Node>();
    Vector<Node> closed = new Vector<Node>();

    int h1Cost(Node node) {
        int cost = 0;
        for (int i = 0; i < node.state.length; i++) {
        if (node.state[i] != i) cost++; }
        return cost;
        }

    int h2Cost(Node node) {
        int cost = 0;
        int state[] = node.state;
        for (int i = 0; i < state.length; i++) {
            int v0 = i, v1 = state[i];
            if (v1 == 0)
                continue;
            int row0 = v0 / 3, col0 = v0 % 3, row1 = v1 / 3, col1 = v1 % 3;
            int c = (Math.abs(row0 - row1) + Math.abs(col0 - col1));
            cost += c;
        }
        return cost;
    }
    /*boleh diubah dengan memakai heuristic h1 atau h2 */
    int hCost(Node node) {
    return h1Cost(node);
    }
    Node getBestNode(Vector nodes) {
        int index = 0, minCost = Integer.MAX_VALUE;
        for (int i = 0; i < nodes.size(); i++) {
            Node node = (Node)nodes.elementAt(i);
            if (node.cost < minCost) {
                minCost = node.cost;
                index = i;
            }
        }
        Node bestNode = (Node)nodes.remove(index);
        return bestNode;
    }

    int getPreviousCost(Node node) {
        int i = open.indexOf(node);
        int cost = Integer.MAX_VALUE;
        if (i != -1) {
            cost = open.get(i).cost;
        } else {
            i = closed.indexOf(node);
            if (i != -1)
                cost = closed.get(i).cost;
        }
        return cost;
    }

    void printPath(Vector path) {
        for (int i = 0; i < path.size(); i++) {
            System.out.print(" " + path.elementAt(i) + "\n");
        }
    }

    void run() {
        Node root = space.getRoot();
        Node goal = space.getGoal();
        Node solution = null;
        open.add(root);
        System.out.print("\nRoot: " + root + "\n\n");
        while (open.size() > 0) {
            Node node = getBestNode(open);
            int pathLength = node.getPath().size();
            closed.add(node);
            if (node.equals(goal)) {
                solution = node;
                break;
            }
            Vector<Node> successors = space.getSuccessors(node);
            for (int i = 0; i < successors.size(); i++) {
                Node successor = successors.get(i);
                int cost = hCost(successor) + pathLength + 1;
                int previousCost;
                previousCost = getPreviousCost(successor);
                boolean inClosed;
                inClosed = closed.contains(successor);
                boolean inOpen = open.contains(successor);
                if (!(inClosed || inOpen) || cost < previousCost) {
                    if (inClosed)
                        closed.remove(successor);
                    if (!inOpen)
                        open.add(successor);
                    successor.cost = cost;
                    successor.parent = node;
                }
            }
        }
        if (solution != null) {
            Vector path = solution.getPath();
            System.out.print("\nSolution found\n");
            printPath(path);
        }
    }

    public static void main(String args[]) {
        new EightPuzzleSearch().run();
    }
    }




## Tugas 

1. Pelajari class EightPuzzleSearch, EightPuzzleSpace, dan Node.
2. Ubahlah initial dan goal state dari program di atas sehingga bentuk initial dan goal statenya  Gambar 8. Kemudian tentukan langkah-langkah mana saja sehingga puzzlenya mencapai goal  state. Analisa dan bedakan dengan solusi pada point 1.
3. Ubahlah initial dan goal state dari program di atas sehingga bentuk initial dan goal statenya Gambar 5.9. Kemudian tentukan langkah-langkah mana saja sehingga puzzlenya mencapai  goal state. Analisa dan bedakan dengan solusi pada point 1 dan 2.
4. Ubahlah initial dan goal state dari program di atas sehingga bentuk initial dan goal statenya  Gambar 5.10. Kemudian tentukan langkah-langkah mana saja sehingga puzzlenya mencapai  goal state. Analisa dan bedakan dengan solusi pada point 1, 2, dan 3.
5. Ubahlah initial dan goal state dari program dan class-class di atas sehingga bentuk initial dan  goal statenya Gambar 5.11. Kemudian tentukan langkah-langkah mana saja sehingga  puzzlenya mencapai goal state.

## Jawaban
## 1. Dalam kode yang diberikan, terdapat tiga kelas utama: `Node`, `EightPuzzleSpace`, dan `EightPuzzleSearch`. Mari kita bahas peran masing-masing kelas:

> `Node`:
   - `Node` adalah kelas yang merepresentasikan satu keadaan dalam permainan teka-teki delapan kotak (Eight Puzzle). Setiap objek `Node` memiliki atribut `state` yang merupakan array integer dengan panjang 9, yang mewakili konfigurasi kotak-kotak pada papan teka-teki.
   - Kelas `Node` memiliki atribut `cost`, yang digunakan untuk menghitung biaya atau nilai fungsi heuristik.
   - `Node` memiliki atribut `parent`, yang merupakan referensi ke `Node` yang menjadi induk dari `Node` saat ini dalam pencarian. Ini digunakan untuk melacak jalur solusi.
   - Atribut `successors` adalah vektor yang berisi semua keadaan yang dapat dijangkau dari `Node` saat ini.
   - Kelas ini memiliki metode seperti `toString`, `equals`, `getPath`, dan lainnya yang digunakan untuk operasi dan pemrosesan data dalam konteks pencarian algoritma A*.

> `EightPuzzleSpace`:
   - `EightPuzzleSpace` adalah kelas yang mengelola ruang pencarian untuk permainan teka-teki delapan kotak. Ini adalah kelas yang menyediakan metode untuk menghasilkan keadaan awal (root) dan tujuan dalam permainan.
   - Metode `getRoot` digunakan untuk mendapatkan keadaan awal dari permainan teka-teki delapan kotak.
   - Metode `getGoal` digunakan untuk mendapatkan keadaan tujuan dari permainan teka-teki delapan kotak.
   - Kelas ini juga memiliki metode `getSuccessors` yang menghasilkan semua keadaan yang dapat dicapai dari suatu keadaan tertentu dalam permainan.

> `EightPuzzleSearch`:
   - `EightPuzzleSearch` adalah kelas yang mengimplementasikan algoritma pencarian A* untuk menemukan solusi dalam permainan teka-teki delapan kotak.
   - Kelas ini memiliki atribut `space` yang merupakan objek dari kelas `EightPuzzleSpace` untuk mengakses ruang pencarian.
   - Atribut `open` dan `closed` adalah vektor yang digunakan untuk melacak keadaan yang sedang diproses dan keadaan yang sudah diproses dalam pencarian.
   - Metode `h1Cost` dan `h2Cost` digunakan untuk menghitung biaya heuristik berdasarkan metode heuristik h1 dan h2 yang berbeda.
   - Metode `hCost` digunakan untuk memilih metode heuristik yang akan digunakan selama pencarian (h1 atau h2).
   - Metode `getBestNode` digunakan untuk mendapatkan `Node` dengan biaya terendah dari vektor `nodes`.
   - Metode `getPreviousCost` digunakan untuk mendapatkan biaya sebelumnya dari suatu `Node`.
   - Metode `run` adalah metode utama yang menjalankan algoritma pencarian A* dan mencari solusi dalam permainan teka-teki delapan kotak.
Dengan demikian, ketiga kelas ini bekerja sama untuk menjalankan pencarian solusi dalam permainan teka-teki delapan kotak menggunakan algoritma A*.

## 2. Langkah-langkah sehingga puzzlenya mencapai goal state.
-Inisialisasi node awal (root) dengan keadaan awal, node tujuan dengan keadaan tujuan, dan dua vektor open dan closed.

-Meletakkan node awal ke dalam vektor open.

-Selama vektor open tidak kosong, Maka:

a. Memilih node dengan biaya terendah dari vektor open. Ini dilakukan dengan menghitung biaya kombinasi f(n) = g(n) + h(n), di mana g(n) adalah biaya yang telah ditempuh dari node awal ke node saat ini, dan h(n) adalah estimasi biaya sisa (heuristik) dari node saat ini ke node tujuan.

b. Menambahkan node yang dipilih ke dalam vektor closed untuk menandai bahwa node ini telah diperiksa.

c. Jika node yang dipilih adalah node tujuan, maka solusi telah ditemukan. menghentikan pencarian dan mencetak jalur dari node awal ke node tujuan.

d. Jika node yang dipilih bukan node tujuan, menghasilkan node-node anak (successors) dari node saat ini dengan melakukan langkah-langkah yang mungkin (geser kotak kosong ke atas, bawah, kiri, atau kanan).

e. Untuk setiap node anak, menghitung f(n) dan periksa apakah node tersebut sudah ada di dalam vektor open atau closed. Jika belum, tambahkan node anak ke dalam vektor open dan tentukan biaya f(n) serta node induknya.

Jika vektor open kosong dan solusi tidak ditemukan, maka permainan 8-Puzzle tidak memiliki solusi.

Jika solusi ditemukan, ouput langkah-langkah dari node awal ke node tujuan yang membentuk solusi.

## 3. Pencarian solusi ini menggunakan algoritma A* dengan dua jenis heuristic, yaitu h1 (misplaced tiles) dan h2 (Manhattan distance). Langkah-langkah untuk mencapai goal state dari initial state pada teka-teki 8-Puzzle bisa dianalisis sebagai berikut:
> Langkah pertama adalah menginisialisasi initial state dan goal state:
   - Initial state: `{1, 5, 3, 4, 6, 8, 2, 7, 0}`
   - Goal state: `{7, 6, 5, 8, 0, 4, 1, 2, 3}`
> Selanjutnya, kode melakukan pencarian solusi dengan algoritma A* dengan menggunakan heuristic h1 (misplaced tiles) atau h2 (Manhattan distance). Dalam kode ini, heuristic yang digunakan secara default adalah h1.
> Algoritma A* akan menjalankan langkah-langkah berikut:
   - Memasukkan initial state ke dalam open list.
   - Mengulang langkah-langkah berikut sampai open list kosong atau goal state ditemukan:
     a. Memilih node dengan cost terendah dari open list.
     b. Memindahkan node ke dalam closed list.
     c. Jika node yang dipilih adalah goal state, maka pencarian selesai, dan solusi ditemukan.
     d. Jika bukan goal state, maka memeriksa semua successors (keturunan) dari node ini.
     e. Memeriksa apakah successor sudah ada di open list atau closed list, dan jika ada, memperbarui cost jika lebih kecil.
     f. Jika successor belum ada di open list atau closed list, maka memasukkan successor ke dalam open list.
> Setelah goal state ditemukan, kode akan mencetak path atau urutan langkah-langkah yang diperlukan untuk mencapai goal state. Ini akan menghasilkan urutan langkah-langkah yang harus diikuti untuk menyelesaikan teka-teki 8-Puzzle.
Perlu diingat bahwa dalam kode tersebut, dapat memilih heuristic yang digunakan untuk pencarian dengan mengganti `hCost(node)` ke dalam metode `EightPuzzleSearch`. Jika Anda ingin menggunakan h2 (Manhattan distance) sebagai heuristic, gantilah `return h1Cost(node)` menjadi `return h2Cost(node)`.


## 4. Langkah langkah kode sampai menemukan hasil
> Langkah pertama adalah menginisialisasi initial state dan goal state:
   - Initial state: `{ 1 2 3 4 5 6 7 8 0}`
   - Goal state: `{ 1 2 3 4 0 5 6 7 8 }`
> Selanjutnya, kode melakukan pencarian solusi dengan algoritma A* dengan menggunakan heuristic h1 (misplaced tiles) atau h2 (Manhattan distance). Dalam kode ini, heuristic yang digunakan secara default adalah h1.
> Algoritma A* akan menjalankan langkah-langkah berikut:
   - Memasukkan initial state ke dalam open list.
   - Mengulang langkah-langkah berikut sampai open list kosong atau goal state ditemukan:
     a. Memilih node dengan cost terendah dari open list.
     b. Memindahkan node ke dalam closed list.
     c. Jika node yang dipilih adalah goal state, maka pencarian selesai, dan solusi ditemukan.
     d. Jika bukan goal state, maka memeriksa semua successors (keturunan) dari node ini.
     e. Memeriksa apakah successor sudah ada di open list atau closed list, dan jika ada, memperbarui cost jika lebih kecil.
     f. Jika successor belum ada di open list atau closed list, maka memasukkan successor ke dalam open list.
> Setelah goal state ditemukan, kode akan mencetak path atau urutan langkah-langkah yang diperlukan untuk mencapai goal state. Ini akan menghasilkan urutan langkah-langkah yang harus diikuti untuk menyelesaikan teka-teki 8-Puzzle seperti berikut :
 1 2 3 4 5 6 7 8 0 
 1 2 3 4 5 6 7 0 8 
 1 2 3 4 5 6 0 7 8 
 1 2 3 0 5 6 4 7 8 
 1 2 3 5 0 6 4 7 8 
 1 2 3 5 6 0 4 7 8 
 1 2 3 5 6 8 4 7 0 
 1 2 3 5 6 8 4 0 7 
 1 2 3 5 0 8 4 6 7 
 1 2 3 0 5 8 4 6 7 
 1 2 3 4 5 8 0 6 7 
 1 2 3 4 5 8 6 0 7 
 1 2 3 4 5 8 6 7 0 
 1 2 3 4 5 0 6 7 8 
 1 2 3 4 0 5 6 7 8

## 5. Langkah langkah untuk menemukan hasil sesuai gambar 5.11
yang pertama ubah kode seperti dibawah


import java.util.Vector;
        
    class Node {
    char[] state = new char[9];
     int cost;
     Node parent = null;
      Vector<Node> successors = new Vector<Node>();

    Node(char s[], Node parent) {
        this.parent = parent;
        for (int i = 0; i < 9; i++)
            state[i] = s[i];
    }

    public Node() {
    }

    public String toString() {
        String s = "";
        for (int i = 0; i < 9; i++) {
            s = s + state[i] + " ";
        }
        return s;
    }

    public boolean equals(Object node) {
        Node n = (Node) node;
        boolean result = true;
        for (int i = 0; i < 9; i++) {
            if (n.state[i] != state[i])
                result = false;
        }
        return result;
    }

    Vector<Node> getPath(Vector<Node> v) {
        v.insertElementAt(this, 0);
        if (parent != null)
            v = parent.getPath(v);
        return v;
    }

    Vector<Node> getPath() {
        return getPath(new Vector<Node>());
    }

    public static void main(String args[]) {
        new Node().run();
    }

    private void run() {
    }
    }

    class EightPuzzleSpace {
     Node getRoot() {
        char initialState[] = { 'D', 'B', 'E', 'A', 'F', 'G', 'H', 'C', 'x' };
        return new Node(initialState, null);
    }

    Node getGoal() {
        char goalState[] = { 'A', 'H', 'G', 'B', 'x', 'F', 'C', 'D', 'E' };
        return new Node(goalState, null);
    }

    Vector<Node> getSuccessors(Node parent) {
        Vector<Node> successors = new Vector<Node>();
        for (int r = 0; r < 3; r++) {
            for (int c = 0; c < 3; c++) {
                if (parent.state[(r * 3) + c] == 'x') {
                    if (r > 0) {
                        successors.add(transformState(r - 1, c, r, c, parent));
                    }
                    if (r < 2) {
                        successors.add(transformState(r + 1, c, r, c, parent));
                    }
                    if (c > 0) {
                        successors.add(transformState(r, c - 1, r, c, parent));
                    }
                    if (c < 2) {
                        successors.add(transformState(r, c + 1, r, c, parent));
                    }
                }
            }
        }
        parent.successors = successors;
        return successors;
    }

    Node transformState(int r0, int c0, int r1, int c1, Node parent) {
        char[] s = parent.state;
        char[] newState = { 'x', 'x', 'x', 'x', 'x', 'x', 'x', 'x', 'x' };
        newState[(r1 * 3) + c1] = s[(r0 * 3) + c0];
        newState[(r0 * 3) + c0] = 'x';
        return new Node(newState, parent);
    }

    public static void main(String args[]) {
        new EightPuzzleSpace().run();
    }

    private void run() {
    }
    }

    class EightPuzzleSearch {
    EightPuzzleSpace space = new EightPuzzleSpace();
    Vector<Node> open = new Vector<Node>();
    Vector<Node> closed = new Vector<Node>();

    int h1Cost(Node node) {
        int cost = 0;
        for (int i = 0; i < node.state.length; i++) {
            if (node.state[i] != 'x')
                cost++;
        }
        return cost;
    }

    int h2Cost(Node node) {
        int cost = 0;
        char state[] = node.state;
        for (int i = 0; i < state.length; i++) {
            char v = state[i];
            if (v != 'x') {
                int row0 = i / 3, col0 = i % 3;
                int row1 = i / 3, col1 = i % 3;
                if (v != 'x') {
                    int c = (Math.abs(row0 - row1) + Math.abs(col0 - col1));
                    cost += c;
                }
            }
        }
        return cost;
    }

    int hCost(Node node) {
        return h1Cost(node);
    }

    Node getBestNode(Vector nodes) {
        int index = 0, minCost = Integer.MAX_VALUE;
        for (int i = 0; i < nodes.size(); i++) {
            Node node = (Node) nodes.elementAt(i);
            if (node.cost < minCost) {
                minCost = node.cost;
                index = i;
            }
        }
        Node bestNode = (Node) nodes.remove(index);
        return bestNode;
    }

    int getPreviousCost(Node node) {
        int i = open.indexOf(node);
        int cost = Integer.MAX_VALUE;
        if (i != -1) {
            cost = open.get(i).cost;
        } else {
            i = closed.indexOf(node);
            if (i != -1)
                cost = closed.get(i).cost;
        }
        return cost;
    }

    void printPath(Vector path) {
        for (int i = 0; i < path.size(); i++) {
            System.out.print(" " + path.elementAt(i) + "\n");
        }
    }

    void run() {
        Node root = space.getRoot();
        Node goal = space.getGoal();
        Node solution = null;
        open.add(root);
        System.out.print("\nRoot: " + root + "\n\n");
        while (open.size() > 0) {
            Node node = getBestNode(open);
            int pathLength = node.getPath().size();
            closed.add(node);
            if (node.equals(goal)) {
                solution = node;
                break;
            }
            Vector<Node> successors = space.getSuccessors(node);
            for (int i = 0; i < successors.size(); i++) {
                Node successor = successors.get(i);
                int cost = hCost(successor) + pathLength + 1;
                int previousCost;
                previousCost = getPreviousCost(successor);
                boolean inClosed;
                inClosed = closed.contains(successor);
                boolean inOpen = open.contains(successor);
                if (!(inClosed || inOpen) || cost < previousCost) {
                    if (inClosed)
                        closed.remove(successor);
                    if (!inOpen)
                        open.add(successor);
                    successor.cost = cost;
                    successor.parent = node;
                }
            }
        }
        if (solution != null) {
            Vector path = solution.getPath();
            System.out.print("\nSolution found\n");
            printPath(path);
        }
    }

    public static void main(String args[]) {
        new EightPuzzleSearch().run();
    }
}


Dengan mengubah nilai int menjadi char yang mana karena puzzle ini kita dari huruf bukan angka makanya type data dibuat char
langkah-langkah yang dilakukan oleh kode  untuk menemukan solusi dari kondisi awal ke kondisi tujuan:

1. Inisialisasi kondisi awal (Root) dan kondisi tujuan (Goal):
   - Kode inisialisasi kondisi awal dan kondisi tujuan dalam kelas `EightPuzzleSpace`. Kondisi awal dan tujuan diwakili oleh larik karakter (char[]).

2. Membuat simpul akar (Root):
   - Simpul akar dibuat menggunakan metode `getRoot` dalam kelas `EightPuzzleSpace`. Ini menginisialisasi larik karakter yang mewakili kondisi awal dan mengembalikan simpul akar dengan larik ini.

3. Menambahkan akar ke daftar terbuka (Open List):
   - Simpul akar ditambahkan ke daftar terbuka (variabel `open`) untuk memulai pencarian.

4. Pencarian A*:
   - Selama daftar terbuka (Open List) tidak kosong, algoritma A* akan terus mencari solusi dengan mengambil simpul terbaik dari daftar terbuka berdasarkan biaya estimasi (fungsi biaya).
   - Kemudian, algoritma akan menghasilkan penerus (successors) dari simpul tersebut dengan menggeser kotak kosong (x) ke atas, bawah, kiri, atau kanan, dan menambahkan penerus ke daftar terbuka jika mereka belum ada di dalamnya.

5. Penentuan Biaya:
   - Terdapat dua jenis biaya yang digunakan dalam kode ini: `h1Cost` dan `h2Cost`. Anda dapat memilih salah satu di antara keduanya dalam metode `hCost` untuk menghitung biaya estimasi.

6. Memeriksa Kebutuhan Perubahan Biaya:
   - Algoritma ini memeriksa apakah biaya estimasi yang dihitung untuk penerus lebih rendah daripada biaya yang telah diketahui sebelumnya. Jika iya, maka simpul tersebut diperbarui dengan biaya yang lebih rendah dan ditambahkan kembali ke daftar terbuka.

7. Berhenti saat Solusi Ditemukan:
   - Algoritma akan berhenti saat simpul yang diambil dari daftar terbuka sama dengan kondisi tujuan (Goal).

8. Menampilkan Solusi:
   - Jika solusi ditemukan, kode akan mencetak langkah-langkah yang diambil untuk mencapai solusi.

Anda dapat menjalankan kode ini dengan menjalankan metode `run` dalam kelas `EightPuzzleSearch`. Jika solusi ada, maka hasilnya akan mencetak langkah-langkah untuk mencapai tujuan. Jika tidak ada solusi, maka pencarian akan terus berlanjut hingga daftar terbuka kosong tanpa menemukan solusi.
