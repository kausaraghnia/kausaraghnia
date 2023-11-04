# Permainan TIC TAC TOE
## Tujuan 
Meningkatkan pemahaman mahasiswa terhadap code permainan tic tac toe. Selain itu, modul 6 memberikan pengetahuan tentang Object Oriented Programming menggunakan bahasa pemrograman salah satu dari java, python, atau C.

## Dasar Teori
Penyelesaian masalah permainan tic tac toe dapat menggunakan algoritma heuristic untuk mencapai solusi yang optimal. Pada modul ini memperlihatkan bagaimana membuat sebuah permainan tic tac toe. Initial state dari permainan ini adalah puzzle ukuran 8 yang tidak berisikan apa-apa. Ketika pemain pertama menekan salah satu ubin, maka ubin tersebut akan diberikan tanda silang. Pemain kedua harus menghalangi pemain pertama untuk membuat tanda silang berjajaran baik secara vertikal, horizontal, atau diagonal. Permainan ini akan berakhir (goal state) ketika salah seorang pemain sudah menderetkan tanda meraka masing-masing baik secara vertikal, horizontal, atau diagonal.
Solusi dari permasalahan ini dapat dilakukan dengan membuat topologi Tree, kemudian setiap langkah dari pemain pertama atau kedua akan menjadikan initial state selanjutnya, kemudian langkah tersebut akan dijadikan sebagai initial state yang baru sampai menemukan goal statenya.Ilustrasi penyelesaian masalah permainan tic tac toe ini dapat dilihat melalui Gambar.

![image](https://github.com/kausaraghnia/kausaraghnia/assets/148691014/da9b2e86-fcd6-4ea2-bd07-02ade07eb673)

## Implementasi Permainan TIC TAC TOE
## Code dan Pejelasan
Dalam tugas ini saya menggunakan bahasa pemrogaman python. Python merupakan pemrograman yang cukup mudah dan simpel sehingga cukup bagus untuk pemula dalam dunia per codingan seperti saya.
    import tkinter as tk
    from tkinter import messagebox

Kode ini mengimpor modul tkinter untuk membangun antarmuka pengguna grafis (GUI) dan modul messagebox untuk menampilkan kotak pesan dalam GUI.

    # Fungsi untuk mengecek apakah ada pemenang atau seri
    def check_winner():
    for i in range(3):
        if board[i][0] == board[i][1] == board[i][2] != "":
            return board[i][0]
        if board[0][i] == board[1][i] == board[2][i] != "":
            return board[0][i]
    if board[0][0] == board[1][1] == board[2][2] != "":
        return board[0][0]
    if board[0][2] == board[1][1] == board[2][0] != "":
        return board[0][2]
    if all(board[i][j] != "" for i in range(3) for j in range(3)):
        return "Draw"
    
    return None

Fungsi ini digunakan untuk memeriksa apakah ada pemenang atau seri dalam permainan Tic-Tac-Toe. Ia melakukan pengecekan baris, kolom, dan diagonal untuk menentukan pemenang. Jika ada pemenang, ia akan mengembalikan "X" atau "O" yang menang; jika tidak, ia akan mengembalikan None.

    # Fungsi untuk menangani klik pada papan permainan
    winner = None
    turn = True

Variabel winner digunakan untuk menyimpan pemenang permainan, sedangkan turn digunakan untuk menentukan giliran pemain (jika turn adalah True, maka giliran pemain saat ini adalah "X", jika False, giliran pemain saat ini adalah "O").

    def on_click(row, col):
    global turn, winner

    if board[row][col] == "" and not winner:
        if turn:
            board[row][col] = "X"
            buttons[row][col].config(text="X", state="disabled")
        else:
            board[row][col] = "O"
            buttons[row][col].config(text="O", state="disabled")

        turn = not turn

        result = check_winner()
        if result:
            winner = result
            messagebox.showinfo("Tic-Tac-Toe", f"Player {winner} wins!")
            for row in buttons:
                for button in row:
                    button.config(state="disabled")


Fungsi ini menangani tindakan klik pada papan permainan. Jika kotak yang diklik kosong dan permainan belum memiliki pemenang, maka kotak tersebut akan diisi dengan "X" atau "O" sesuai giliran pemain, dan tombol akan dinonaktifkan. Kemudian, fungsi check_winner() dipanggil untuk memeriksa apakah ada pemenang. Jika ada pemenang, pesan pemenang akan ditampilkan dalam kotak pesan, dan semua tombol akan dinonaktifkan.

    # Fungsi untuk me-reset permainan
    def reset_game():
    global board, turn, winner
    board = [["" for _ in range(3)] for _ in range(3)]
    turn = True
    winner = None
    for row in buttons:
        for button in row:
            button.config(text="", state="normal")

Fungsi ini digunakan untuk mereset permainan. Semua variabel global diatur ulang, papan permainan dikosongkan, dan tombol-tombol dikembalikan ke keadaan awal.

    # Membuat jendela utama
    root = tk.Tk()
    root.title("Tic-Tac-Toe")

Kode ini membuat jendela utama dengan judul "Tic-Tac-Toe".

    # Membuat papan permainan
    board = [["" for _ in range(3)] for _ in range(3)]
    buttons = [[None for _ in range(3)] for _ in range(3)]
    for i in range(3):
        for j in range(3):
            buttons[i][j] = tk.Button(root, text="", font=("normal", 20), width=5, height=2,
                                 command=lambda i=i, j=j: on_click(i, j))
            buttons[i][j].grid(row=i, column=j)

Kode ini membuat papan permainan 3x3 dengan tombol-tombol yang dapat diklik. Setiap tombol memiliki teks awal kosong dan dikonfigurasi dengan fungsi on_click() untuk menghandle klik.

    # Membuat tombol reset
    reset_button = tk.Button(root, text="Reset Game", font=("normal", 16), command=reset_game)
    reset_button.grid(row=3, columnspan=3)

Kode ini membuat tombol "Reset Game" yang memanggil fungsi reset_game() saat diklik.

    # Menjalankan GUI
    root.mainloop()
Kode ini memulai loop utama GUI tkinter, sehingga GUI akan berjalan dan menunggu input pengguna.

Setelah itu program di run nanti akan menghasilkan output GUI TicTacToe seperti dibawah.

![image](https://github.com/kausaraghnia/kausaraghnia/assets/148691014/2b8fd1b9-ab19-4796-a2e9-163896608176)

Untuk klik pada kolom nanti klik pertama menampilkan X dan klik ke 2 menampilkan O sampai ada X atau O yang sebaris baik vertikal, diagonal dan horizontal. contoh hasil permainan seperti dibawah.

![image](https://github.com/kausaraghnia/kausaraghnia/assets/148691014/e1ea7718-78c3-4cd1-872f-aaaf589b2deb) ![image](https://github.com/kausaraghnia/kausaraghnia/assets/148691014/1ef667a2-ea1f-40d9-94d2-52381a3b4d32)

Klik tombol reset maka nanti tampilan akan kosong lagi seperti gambar tampilan pertama.
