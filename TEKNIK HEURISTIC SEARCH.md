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



