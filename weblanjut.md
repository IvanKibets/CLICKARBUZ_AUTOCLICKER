## JAWABAN UAS WEB LANJUT
### Aditya Saputra 201843500528


1.
```mysql

A.  CREATE TABLE buku (
    kode_buku varchar(5),
    judul_buku varchar(30),
    harga int(5),
    tanggal_terbit date,
    stok int(4)
);

B.  CREATE TABLE pinjam (
    kode_pinjam varchar(5),
    kode_buku varchar(5),
    nama_mahasiswa varchar(30),
    tanggal_pinjam date,
    tanggal_kembali date,
);

C.  CREATE DATABASE adityasaputra_S6G; 

```


2.
```php
    <?php
    
    $databaseHost = 'localhost';
    $databaseName = 'adityasaputra_S6G';
    $databaseUsername = 'root';
    $databasePassword = 'dev123';
    
    $mysqli = mysqli_connect($databaseHost, $databaseUsername, $databasePassword, $databaseName); 
    
    ?>
```

3. ```php
    //index.php (Select/Read)

    <?php
    // Create database connection using config file
    include_once("koneksi.php");
    
    // Fetch all users data from database
    $result = mysqli_query($mysqli, "SELECT * FROM buku");
    ?>
    
    <html>
    <head>    
        <title>Homepage</title>
    </head>
    
    <body>
    <a href="tambah.php">Tambahkan Buku</a><br/><br/>
    
        <table width='80%' border=1>
    
        <tr>
            <th>KODE BUKU</th> <th>JUDUL BUKU</th> <th>HARGA</th> <th>TANGGAL TERBIT</th> <th>STOK</th>
        </tr>
        <?php  
        while($user_data = mysqli_fetch_array($result)) {         
            echo "<tr>";
            echo "<td>".$user_data['kode_buku']."</td>";
            echo "<td>".$user_data['judul_buku']."</td>";
            echo "<td>".$user_data['harga']."</td>";    
            echo "<td>".$user_data['tanggal_terbit']."</td>";    
            echo "<td>".$user_data['stok']."</td>";    
            echo "<td><a href='rubah.php?id=$user_data[id]'>Rubah</a> | <a href='hapus.php?id=$user_data[id]'>Hapus</a></td></tr>";        
        }
        ?>
        </table>
    </body>
    </html>



    //tambah.php (Create/Tambahdata)
    <html>
    <head>
        <title>Tambah Buku</title>
    </head>
    
    <body>
        <a href="index.php">Kembali Ke halaman utama</a>
        <br/><br/>
    
        <form action="tambah.php" method="post" name="form1">
            <table width="25%" border="0">
                <tr> 
                    <td>ID</td>
                    <td><input type="text" name="id"></td>
                </tr>
                <tr> 
                    <td>KODE BUKU</td>
                    <td><input type="text" name="kode_buku"></td>
                </tr>
                <tr> 
                    <td>JUDUL BUKU</td>
                    <td><input type="text" name="judul_buku"></td>
                </tr>
                <tr> 
                    <td>HARGA</td>
                    <td><input type="text" name="harga"></td>
                </tr>
                <tr> 
                    <td>TANGGAL TERBIT</td>
                    <td><input type="date" name="tanggal_terbit"></td>
                </tr>
                <tr> 
                    <td>STOK</td>
                    <td><input type="text" name="stok"></td>
                </tr>
                <tr> 
                    <td></td>
                    <td><input type="submit" name="Submit" value="tambah"></td>
                </tr>
            </table>
        </form>
        
        <?php
    
        // Check If form submitted, insert form data into users table.
        if(isset($_POST['Submit'])) {
            $id = $_POST['id'];
            $kode_buku = $_POST['kode_buku'];
            $judul_buku = $_POST['judul_buku'];
            $harga = $_POST['harga'];
            $tanggal_terbit = $_POST['tanggal_terbit'];
            $stok = $_POST['stok'];
            // include database connection file
            include_once("koneksi.php");
                    
            // Insert user data into table
            $result = mysqli_query($mysqli, "INSERT INTO buku( id, kode_buku,judul_buku,harga,tanggal_terbit,stok ) VALUES('$id', '$kode_buku','$judul_buku','$harga','$tanggal_terbit','$stok')");
            
            echo "<script>console.log('Hasillllll: " . $result . "' );</script>";
            

            // Show message when user added
            echo "Buku berhasil di tambahkan. <a href='index.php'>Lihat Buku</a>";
        }
        ?>
    </body>
    </html>


    //rubah.php
    <?php
    // include database connection file
    include_once("koneksi.php");
    
    // Check if form is submitted for user update, then redirect to homepage after update
    if(isset($_POST['update']))
    {	
        $id = $_POST['id'];
            $kode_buku = $_POST['kode_buku'];
            $judul_buku = $_POST['judul_buku'];
            $harga = $_POST['harga'];
            $tanggal_terbit = $_POST['tanggal_terbit'];
            $stok = $_POST['stok'];
            
        // update user data
        $result = mysqli_query($mysqli, "UPDATE buku SET kode_buku='$kode_buku',judul_buku='$judul_buku',harga='$harga', tanggal_terbit='$tanggal_terbit', stok='$stok' WHERE id=$id");
        
        // Redirect to homepage to display updated user in list
        header("Location: index.php");
    }
    ?>
    <?php
    // Display selected user data based on id
    // Getting id from url
    $id = $_GET['id'];
    
    // Fetech user data based on id
    $result = mysqli_query($mysqli, "SELECT * FROM buku WHERE id=$id");
    
    while($user_data = mysqli_fetch_array($result))
    {
        $kode_buku = $user_data['kode_buku'];
        $judul_buku = $user_data['judul_buku'];
        $harga =  $user_data['harga'];  
        $tanggal_terbit = $user_data['tanggal_terbit'];
        $stok = $user_data['stok']; 
    }
    ?>
    <html>
    <head>	
        <title>Rubah Data Buku</title>
    </head>
    
    <body>
        <a href="index.php">Home</a>
        <br/><br/>
        
        <form name="update_buku" method="post" action="rubah.php">
            <table border="0">
                <tr> 
                    <td>KODE BUKU</td>
                    <td><input type="text" name="kode_buku" value=<?php echo $kode_buku;?>></td>
                </tr>
                <tr> 
                    <td>JUDUL BUKU</td>
                    <td><input type="text" name="judul_buku" value=<?php echo $judul_buku;?>></td>
                </tr>
                <tr> 
                    <td>HARGA</td>
                    <td><input type="text" name="harga" value=<?php echo $harga;?>></td>
                </tr>
                <tr> 
                    <td>TANGGAL TERBIT</td>
                    <td><input type="date" name="tanggal_terbit" value=<?php echo $tanggal_terbit;?>></td>
                </tr>
                <tr> 
                    <td>STOK</td>
                    <td><input type="text" name="stok" value=<?php echo $stok;?>></td>
                </tr>
                <tr>
                    <td><input type="hidden" name="id" value=<?php echo $_GET['id'];?>></td>
                    <td><input type="submit" name="update" value="Update"></td>
                </tr>
            </table>
        </form>
    </body>
    </html>

    //hapus.php
    <?php
    // include database connection file
    include_once("koneksi.php");
    
    // Get id from URL to delete that user
    $id = $_GET['id'];
    
    // Delete user row from table based on given id
    $result = mysqli_query($mysqli, "DELETE FROM buku WHERE id=$id");
    
    // After delete redirect to Home, so that latest user list will be displayed.
    header("Location:index.php");
    ?>

    // 4.

    <html>
        <head>
            <title>Input</title>
        </head>

        <body>
            <br/><br/>

            <form action="index.php" method="post" name="form1">
            <h1>Input</h1>
                <table width="25%" border="0">
                    <tr>
                        <td>ID</td>
                        <td><input type="text" name="id"></td>
                    </tr>
                    <tr>
                        <td>NPM</td>
                        <td><input type="text" name="npm"></td>
                    </tr>
                    <tr>
                        <td>TUGAS</td>
                        <td><input type="text" name="tugas"></td>
                    </tr>
                    <tr>
                        <td>UTS</td>
                        <td><input type="text" name="uts"></td>
                    </tr>
                    <tr>
                        <td>UAS</td>
                        <td><input type="text" name="uas"></td>
                    </tr>
                    <tr>
                        <td>Mata Kuliah</td>
                        <td>
                            <select name="matkul" id="matkul">
                            <option value="v">Pemprograman Visual</option>
                            <option value="w">Pemprograman Web</option>
                            </select>
                        </td>
                    </tr>
                    <tr>
                        <td></td>
                        <td><input type="submit" name="Submit" value="tambah"></td>
                    </tr>
                </table>
            </form>

            <?php

            echo "<strong>Output</strong><br>";

            if (isset($_POST['Submit'])) {
                $id = $_POST['id'];
                $npm = $_POST['npm'];
                $tugas = $_POST['tugas'];
                $uts = $_POST['uts'];
                $uas = $_POST['uas'];
                // include database connection file
                include_once("koneksi.php");

                // Insert user data into table
                $result = mysqli_query($mysqli, "INSERT INTO tbl_nilai( id, npm,tugas,uts,uas ) VALUES('$id', '$npm', '$tugas','$uts','$uas')");

                if ($npm) {
                    echo "<strong>NPM :</strong> {$npm} <br>";
                } else if ($tugas) {
                    echo "<strong>Tugas:</strong> {$tugas} <br>";
                } else if ($uts) {
                    echo "<strong>UTS:</strong> {$uts} <br>";
                } else if ($uas) {
                    echo "<strong>UAS:</strong> {$uas} <br>";
                } 

            }

            

        ?>
        </body>
        </html>
```
