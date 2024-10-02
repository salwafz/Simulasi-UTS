# Aplikasi CRUD Entitas Buku Dengan Menggunakan Jawa Swing dan Database PostgreSQL

## Deskripsi Penugasan
Pada Projek Simulasi UTS Pemrograman Berbasis Objek (PBO) ini, diminta untuk menyelesaikan soal soal yaitu mengimplementasikan penggunaan CRUD pada Netbeans menggunakan bahasa pemrograman Java dan database PostgreSQL. Penggunaan CRUD kali ini akan dibangun dengan bantuan Java Swing untuk mendukung operasi CRUD tersebut.

Langkah-langkah Membuat CRUD dengan Java :
### 1. Membuat database baru pada PostgreSQL.
### 2. Membuat tabel baru dengan atribut yang telah ditentukan.
### 3. Membuat Project baru pada netbeans.
### 4. Membuat file JFrame Form pada Project Package yang telah dibuat.
### 5. Mengkoneksikan PostgreSQL dengan java menggunakan JDBC.
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

### 6. Membuat User Inferface GUI menggunakan Java Swing
Buat tampilan aplikasi menggunakan Java Swing dengan beberapa fitur yang tersedia,
- **Label**, untuk membuat text baik judul maupun keterangan lainnya,
 (DATA BUKU, ISBN, JUDUL BUKU, TAHUN TERBIT, PENERBIT).
- **Text Field**, membuat wadah/tempat untuk proses menginput data,
  (txtISBN, txtJudul, txtThnTerbit, txtPenerbit).
- **Button**, untuk membuat tombol pengoprasian pada aplikasi nantinya,
  (btnBaru, btnSimpan, btnupdate, btnDelete, btnxit).
- **Table**, untuk membuat tabel pada aplikasi yang nantinya akan digunakan untuk menampilkan hasil dari transaksi.
Berikut adalah contoh tampilan GUI yang akan digunakan :
![image](https://github.com/user-attachments/assets/64dafd18-84c8-4b55-986e-4ea1c59eab6d)

### 7. Menampilkan Data Utama 
Pada file java, buat fungsi method tampil untuk menampilkan data dalam tabel pada saat aplikasi dijalankan pertama kali :
<pre>
      public void tampil() {
        try {
            // TODO code application logi
            Class.forName(driver);
            String sql = "SELECT * FROM entitasBuku";
            conn = DriverManager.getConnection(koneksi, user, password);
            stmt = conn.createStatement();

            while (!conn.isClosed()) {

                ResultSet rs = stmt.executeQuery(sql);
                this.tblHasil.setModel(pertemuankelima.DbUtils.resultSetToTableModel(rs));
                while (rs.next()) {
                    System.out.println(String.valueOf(rs.getObject(1)) + " "
                            + String.valueOf(rs.getObject(2)) + " "
                            + String.valueOf(rs.getObject(3)) + " " + String.valueOf(rs.getObject(4)));
                }
                conn.close();
            }

            stmt.close();

        } catch (ClassNotFoundException ex) {
            Logger.getLogger(BayuFrame.class.getName()).log(Level.SEVERE, null, ex);
        } catch (SQLException ex) {
            Logger.getLogger(BayuFrame.class.getName()).log(Level.SEVERE, null, ex);
        }
</pre>
Code dibawah **OPTIONAL** untuk ditambahkan (fungsi : mengatur interaktif button dan text field saat digunakan). Jika tidak digunakan, langsung akhiri dengan **}** pada akhir code.
<pre>
        btnSimpan.setEnabled(false);
        txtISBN.setEnabled(false);
        txtJudul.setEnabled(false);
        txtThnTerbit.setEnabled(false);
        txtPenerbit.setEnabled(false);
    }
</pre>

### 8. Membuat Method bersih()
Dibawah method tampil, buat method bersih(), berfungsi untuk membersihkan text field pada saat transaksi berhasil dilakukan secara otomatis.
- Masukkan code dibawah ini :
  <pre>
    public void bersih() {
        txtISBN.setText("");
        txtJudul.setText("");
        txtThnTerbit.setText("");
        txtPenerbit.setText("");
    }
  </pre>

### 9. Penggunaan **btnBaru**
**Button Baru** ini digunakan untuk membuat atau memasukkan data baru pada text field, tombol ini akan berfungsi hanya jika pengguna menekan tombol ini untuk memasukkan data yang baru (apabila tidak di tekan, semua text field dinonaktifkan atau tidak bisa digunakan).
- Tambahkan code berikut, untuk mengaktifkan penggunaan **btnBaru**
  <pre>
    private void btnBaruActionPerformed(java.awt.event.ActionEvent evt) {                                        
        // TODO add your handling code here:
        txtISBN.setText("");
        txtJudul.setText("");
        txtThnTerbit.setText("");
        txtPenerbit.setText("");

        btnUpdate.setEnabled(false);
        btnDelete.setEnabled(false);
        btnSimpan.setEnabled(true);
        txtISBN.setEnabled(true);
        txtJudul.setEnabled(true);
        txtThnTerbit.setEnabled(true);
        txtPenerbit.setEnabled(true);
    }
  </pre>

### 10. Penggunaan **btnSimpan**
**Button Simpan** ini nantinya akan digunakan untuk transaksi menyimpan data yang baru dimasukkan atau diinputkan.
- Tambahkan code dibawah untuk membuat fungsi dari button tsb :
  <pre>
    if (txtISBN.getText().equals("")) {
            JOptionPane.showMessageDialog(null, "Isi semua data");
        } else if (txtJudul.getText().equals("")) {
            JOptionPane.showMessageDialog(null, "Isi semua data");
        } else if (txtThnTerbit.getText().equals("")) {
            JOptionPane.showMessageDialog(null, "Isi semua data");
        } else if (txtPenerbit.getText().equals("")) {
            JOptionPane.showMessageDialog(null, "Isi semua data");
        } else {
            try {
                Class.forName(driver);
                conn = DriverManager.getConnection(koneksi, user, password);
                conn.setAutoCommit(false);

                String sql = "INSERT INTO entitasBuku VALUES(?,?,?,?)";
                pstmt = conn.prepareStatement(sql);

                String isbn, judul_buku, tahun_terbit, penerbit;
                isbn = txtISBN.getText();
                judul_buku = txtJudul.getText();
                tahun_terbit = txtThnTerbit.getText();
                penerbit = txtPenerbit.getText();

                pstmt.setString(1, isbn);
                pstmt.setString(2, judul_buku);
                pstmt.setLong(3, Long.parseLong(tahun_terbit));
                pstmt.setString(4, penerbit);

                pstmt.executeUpdate();
                conn.commit();
                pstmt.close();
                conn.close();
                bersih();

                JOptionPane.showMessageDialog(null, "Data Berhasil Ditambahkan");
            } catch (ClassNotFoundException | SQLException ex) {
                JOptionPane.showMessageDialog(null, "Terjadi Kesalahan Saat Pengisian Data");
            }
        }
        tampil();
    }              
  </pre>
  Pada code akhir, ditambahkan **tampil()** untuk memanggil method tampil() agar hasil dari transaksi langsung bisa ditampilkan kedalam tabel.
  <pre>
    }
        tampil();
    }   
  </pre>

### 11. Mengaktifkan Event MouseClicked pada Tabel
- Pada design Java Swing, klik kanan pada tabel.
- Pilih **Events > Mouse > mouseClicked**.
- Otomatis akan diarahkan pada code untuk fungsi tsb, masukkan code dibawah :
  <pre>
      private void tblHasilMouseClicked(java.awt.event.MouseEvent evt) {                                      
        // TODO add your handling code here:
        int row = tblHasil.getSelectedRow();
        txtISBN.setText(tblHasil.getValueAt(row, 0).toString());
        txtJudul.setText(tblHasil.getValueAt(row, 1).toString());
        txtThnTerbit.setText(tblHasil.getValueAt(row, 2).toString());
        txtPenerbit.setText(tblHasil.getValueAt(row, 3).toString());

    </pre>
    Code dibawah **OPTIONAL** untuk ditambahkan.
    <pre>
        btnSimpan.setEnabled(false);
        btnUpdate.setEnabled(true);
        btnDelete.setEnabled(true);
        txtISBN.setEnabled(true);
        txtJudul.setEnabled(true);
        txtThnTerbit.setEnabled(true);
        txtPenerbit.setEnabled(true);
    }  
    </pre>

### 12. Penggunaan btnUpdate
Pada **Button Update** ini, akan menggunakan Events MouseClicked pada tabel untuk mengakses data agar dapat diedit.
- Klik 2 kali pada design GUI pada **BUTTON UPDATE**, otomatis akan diarahkan pada lokasi code tsb.
- Masukkan code dibawag ini :
  <pre>
    private void btnUpdateActionPerformed(java.awt.event.ActionEvent evt) {                                          
        // TODO add your handling code here:
        String isbn, judul_buku, tahun_terbit, penerbit;
        if (txtISBN.getText().equals("")) {
            JOptionPane.showMessageDialog(null, "Isi semua data");
        } else if (txtJudul.getText().equals("")) {
            JOptionPane.showMessageDialog(null, "Isi semua data");
        } else if (txtThnTerbit.getText().equals("")) {
            JOptionPane.showMessageDialog(null, "Isi semua data");
        } else if (txtPenerbit.getText().equals("")) {
            JOptionPane.showMessageDialog(null, "Isi semua data");
        } else {
            try {
                Class.forName(driver);
                String sql = "UPDATE entitasBuku SET judul_buku = ?, tahun_terbit = ?, penerbit = ? WHERE isbn = ?";
                conn = DriverManager.getConnection(koneksi, user, password);
                pstmt = conn.prepareStatement(sql);

                isbn = txtISBN.getText();
                judul_buku = txtJudul.getText();
                tahun_terbit = txtThnTerbit.getText();
                penerbit = txtPenerbit.getText();

                pstmt.setString(1, judul_buku);
                pstmt.setLong(2, Long.parseLong(tahun_terbit));
                pstmt.setString(3, penerbit);
                pstmt.setString(4, isbn);

                int rowsAffected = pstmt.executeUpdate();
                if (rowsAffected > 0) {
                    JOptionPane.showMessageDialog(null, "Data Berhasil Diupdate");
                    pstmt.close();
                    conn.close();
                    bersih();
                } else {
                    JOptionPane.showMessageDialog(null, "Data Tidak Ditemukan");
                }
            } catch (ClassNotFoundException | SQLException ex) {
                Logger.getLogger(DataBuku.class.getName()).log(Level.SEVERE, null, ex);
            }
        }
        tampil();
    }     
  </pre>

### 13. Penggunaan btnDelete
Pada **Button Delete** ini, akan menggunakan Events MouseClicked pada tabel untuk mengakses data agar dapat diedit.
- Klik 2 kali pada design GUI **BUTTON DELETE**, otomatis akan diarahkan pada lokasi code tsb.
- Masukkan code dibawag ini :
  <pre>
    private void btnDeleteActionPerformed(java.awt.event.ActionEvent evt) {                                          
        // TODO add your handling code here:
        String isbn;
        isbn = txtISBN.getText();

        try {
            Class.forName(driver);
            conn = DriverManager.getConnection(koneksi, user, password);

            int jawab = JOptionPane.showConfirmDialog(null, "Apakah Anda Yakin Akan Menghapus Data Ini?");
            switch (jawab) {
                case JOptionPane.YES_OPTION:
                    String deleteSql = "DELETE FROM entitasBuku WHERE isbn = ?";
                    pstmt = conn.prepareStatement(deleteSql);
                    pstmt.setString(1, isbn);
                    pstmt.executeUpdate();
                    pstmt.close();
                    conn.close();
                    bersih();
                    break;
                case JOptionPane.NO_OPTION:
                    JOptionPane.showMessageDialog(this, "Data Tidak Jadi Dihapus");
                    break;
            }
        } catch (ClassNotFoundException | SQLException ex) {
            JOptionPane.showMessageDialog(null, "Periksa Kembali");
        }
        tampil();
    }  
  </pre>

### 14.Penggunaan btnExit
Pada **Button Exit** ini, berfungsi untuk keluar dari program, sekaligus menghentikan program yang sedang berjalan.
-Klik 2 kali pada design GUI **BUTTON EXIT**, otomatis akan diarahkan pada lokasi code tsb.
- Masukkan code dibawag ini :
  <pre>
    private void btnExitActionPerformed(java.awt.event.ActionEvent evt) {                                        
        // TODO add your handling code here:
        System.exit(0);
    } 
  </pre>

### 15. Penggunaan btnBersih
**BUTTON BERSIH** ini digunakan untuk membersihkan text field apabila tidak jadi melakukan pengeditan pada data tsb.
-Berikut adalah code untuk button bersih :
  <pre>
     private void btnBersihActionPerformed(java.awt.event.ActionEvent evt) {                                          
        // TODO add your handling code here:
        txtISBN.setText("");
        txtJudul.setText("");
        txtThnTerbit.setText("");
        txtPenerbit.setText("");
    }  
  </pre>

## DEMO PENGGUNAAN APLIKASI CRUD ENTITAS BUKU
### 1. Tampilan Utama
Output program yang dihasilkan pada saat running. Text File dinonaktifkan.
![image](https://github.com/user-attachments/assets/98ed6534-cc4c-41b9-b959-dc827a4d3fc2)

### 2. Tampilan btnBaru
Pada button ini, akan mengaktifkan text field untuk pengisian data baru.
![image](https://github.com/user-attachments/assets/4dc20e15-7c2b-49f6-b041-68ed74bf80fd)

### 3. Tampilaan dan Fungsi btnSimpan
- Setelah data dimasukkan, data akan disimpan dengan menekan button simpan.
- Jika data berhasil disimpan, akan ada PopUp untuk menginformasikan jika data berhasil tersimpan.
  ![image](https://github.com/user-attachments/assets/1c3df393-2ede-4917-a2dd-88e01a92f00b)

### 4. Tampilan dan Fungsi btnUpdate
- Klik data pada tabel.
  ![image](https://github.com/user-attachments/assets/484cde2d-8df5-4141-9e0f-9c4bc482c8d1)
- Ubah data sesuai yang diingkan (mengubah judul buku yang baru ditambahkan sebelumnya).
  ![image](https://github.com/user-attachments/assets/ce86fe5b-dc1f-4b38-b3d7-bbd20b96b06a)
- Kemudian klik button update untuk merubah data dan menyimpan data yang baru.
- Jika berhasil, akan ada PopUp untuk mengkonfirmasikan jika data berhasil terupdate.
  ![image](https://github.com/user-attachments/assets/1ebfce95-76b1-4cc2-887a-922af90de721)
  

### 5. Tampilan dan Fungsi btnDelete
- Klik data pada tabel.
    ![image](https://github.com/user-attachments/assets/2d918f9c-6059-4fa2-a5fb-9b8e7cb8c1cb)
- Kemudian klik button delete untuk menghapus data.
- Jika data berhasil terhapus, akan muncul PopUp untuk mengkonfirmasi apakah data benar akan dihapus.
  ![image](https://github.com/user-attachments/assets/4e68d4a5-6f92-40b8-8759-b5a6c57a8505)









  

  










