# Aplikasi CRUD Entitas Buku Dengan Menggunakan Jawa Swing dan Database PostgreSQL

## Deskripsi Penugasan
Pada Projek Simulasi UTS Pemrograman Berbasis Objek (PBO) ini, diminta untuk menyelesaikan soal soal yaitu mengimplementasikan penggunaan CRUD pada Netbeans menggunakan bahasa pemrograman Java dan database PostgreSQL. Penggunaan CRUD kali ini akan dibangun dengan bantuan Java Swing untuk mendukung operasi CRUD tersebut.

## Koneksi Database
Aplikasi ini nantinya akan terhubung ke database menggunakan JDBC, dengan library PostgreSQL yang diperlukan untuk mengelola operasi CRUD. Jadi pastikan anda telah membuat database terlebih dahulu menggunakan PostgreSQL dengan struktur yang sesuai dan jelas. Seperti yang digunakan pada projek kali ini yakni nim, nama_mahasiswa, alamat dan no_telepon.

## Connecting to Postgres With Java using JDBC
- Sebelumnya, buatlah database pada PostgreSQL dengan ketentuan sebagai berikut : (atribut : ISBN, Judul Buku, Tahun Terbit, Penerbit! )
- Tambahkan library dan koneksikan database pada netbeans.
- Buat file Jframe Form baru, dan beri nama **entitasBuku**.
- Pada file java entitasBuku, masukkan code berikut untuk mengkoneksikan database dengan Aplikasi yang akan anda buat.
Berikut adalah code java untuk menghubungkan dabatabe PostgreSQL menggunakan JDBC :
<pre>
  Connection conn;
    Statement stmt;
    PreparedStatement pstmt;
    
    String driver = "org.postgresql.Driver";
    String koneksi = "jdbc:postgresql://localhost:5432/nama_database";
    String user = "postgres";
    String password = "******";
    
    InputStreamReader inputStreamReader = new InputStreamReader(System.in);
    BufferedReader input = new BufferedReader(inputStreamReader);
</pre>

## Pembuatan User Inferface GUI menggunakan Java Swing








