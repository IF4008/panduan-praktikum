# Integrating Slim Framework and MySQL with Ionic

Sebelum lanjut ke praktikum silahkan temen-temen simak dulu sedikit INTRO materi yang akan kita pelajari sekarang. 🎥

[![intro papk 8](https://res.cloudinary.com/marcomontalbano/image/upload/v1639575563/video_to_markdown/images/youtube--wjJT80L7hXU-c05b58ac6eb4c4700831b2b3070cd403.jpg)](https://youtu.be/wjJT80L7hXU "intro papk 8")

- [Intergating Slim Framework and MySQL with Ionic](#intergating-slim-framework-and-mysql-with-ionic)
  - [Step 1: Setup Database](#step-1-setup-database)
  - [Step 2: Installasi Local Web Server dan Composer](#step-2-installasi-local-web-server-dan-composer)
  - [Step 3: Installasi Slim framework](#step-3-installasi-slim-framework)
  - [Step 4: Test Web Service](#step-4-test-web-service)
  - [Referensi](#referensi)

What we'll be building?

Kita akan membuat produk penjualan sederhana, di mana pengguna dapat memasukkan informasi produk seperti judul, qty, dan harga.
untuk bagian backend/webservice kita akan menggunakan Micro framework for PHP yaitu [Slim](https://www.slimframework.com) dan untuk databasenya menggunakan MySQL.

Untuk panduan praktikum saya bagi menjadi 4 bagian.

## Step 1: Setup Database

Pertama kita buat database. Buat database `penjualan` dan satu tabel bernama `barang`.

Tabel `Penjualan`:

```SQL
CREATE DATABASE penjualan;
USE penjualan;
CREATE TABLE `barang`(
`id_barang` int(11) NOT NULL AUTO_INCREMENT PRIMARY KEY,
`nama_barang` varchar(255),
`qty` int(11) ,
`harga` int(11)
);
```

![db](images/Screen%20Shot%202021-11-26%20at%2020.04.06.png)

Setelah membuat database dan tabel barang kemudian tambahkan 1 dumy data barang:

![dumy](images/Screen%20Shot%202021-11-29%20at%2020.43.50.png)

![dumy2](images/Screen%20Shot%202021-11-29%20at%2020.39.24.png)

selanjutnya kita akan membuat sebuah `web service` pertukaran data antara `client` dan `server` untuk menangani `request data` `get` dan `post` dari aplikasi ionic yang akan kita buat. Disini kita akan menggunakan [Slimframework](https://www.slimframework.com).

Ok, sedikit saya bahas tentang [Slim framework](https://www.slimframework.com)

Slim Framework adalah mikro framework PHP untuk membuat aplikasi web. Biasanya digunakan untuk membuat `REST API atau web serivice.`

Berlainan dengan full-stack framework yang mengakomodasi banyak hal seperti :

1. account management
2. autentikasi
3. database abstraction
4. templating (html). dan lain sebagainnya

micro framework tidak mengakomodasi fitur-fitur diatas melainkan hanya suatu fungsionalitas kecil. Keuntungannya adalah framework tersebut secara teori tentunnya “lebih ringan” jika dibandingkan dengan full-stack framework atau juga dapat disebut sebagai enterprise framework.

Dan sekali lagi menurut teori, jika framework tersebut ringan otomatis performa yang dihasilkan juga akan lebih baik jika dibandingkan dengan full-stack framework untuk fungsionalitas tertentu (pada kasus ini adalah untuk membangun RESt-API).

**Konsep Slim Framwork**

Slim Framework sebenarnya sangat sederhana. Dia hanya bertugas menerima HTTP Request, lalu memanggil fungsi yang diinginkan. Setelah itu mengembalikan HTTP Response.

Teman-teman bisa baca dokumentasinya untuk lebih jelasnya mengenai [Slim framework](https://www.slimframework.com) // [Documentation](https://www.slimframework.com/docs/v3/).

## Step 2: Installasi Local Web Server dan Composer

Sebelum melakukan instalasi Slim Framework , ada 2 tools yang harus anda disiapkan:

- [x] Install Local Web Server

Untuk `local web server` anda bisa gunakan [Laragon](https://laragon.org/download/index.html) atau [Xampp](https://www.apachefriends.org/index.html).

- [x] Install Composer

Buka link [disini](https://getcomposer.org/Composer-Setup.exe) untuk mengunduh file composer.

Setelah anda menginstall `local web server`. Slim menggunakan `package manager` berupa `composer`, oleh karena itu kita perlu menginstall `composer`.

Setelah selesai download file `Composer`, buka file tersebut dan ikuti instruksi instalasinya.

Setelah proses instalasi Composer selesai, langkah selanjutnya adalah cek instalasi Composer menggunakan Command Prompt (CMD) atau git bash untuk pengguna windows.

![composer](images/Screen%20Shot%202021-11-26%20at%2021.19.53.png)

Apabila muncul seperti gambar di atas, instalasi Anda berhasil. 🎉

## Step 3: Installasi Slim framework

Selanjutnya masuk ke direktori local web server: untuk `xampp` biasanya di direktori `C:\Xampp`

```directori stucture

C:\xampp\htdoc\nama-project     # Document Root (where you store your projects)

```

Untuk direktori `Laragon` juga hampir sama `C:\laragon`

```directori structure

C:\laragon\wwww\nama-project     # Document Root (where you store your projects)

```

Buat folder baru dengan nama folder `webservice-ionic` di dalam document root kemudian open with visual studio code.

![ws](images/Screen%20Shot%202021-11-26%20at%2021.57.01.png)

Kemudian ketikan perintah berikut:

```php
composer require slim/slim "^3.12"
```

Perintah di atas adalah perintah untuk installasi `Slimframework`

Berikut struktur file `slimframework` yang sudah terinstal di direktori:

![slim](images/Nov-29-2021%2020-17-59.gif)

Selanjutnya buat file `config.php` di dalam direktori root anda misal `xampp/htdocs/webservice-ionic`. silahkan disesuaikan dengan local webserver yang kalian install untuk root direktorinya.

File `config.php` ini fungsinya untuk menghubungkan `database penjualan` yang sebelumnya sudaha kita buat.

```php
<?php
error_reporting(0);
session_start();

define('DB_SERVER', 'localhost');
define('DB_USERNAME', 'root');
define('DB_PASSWORD', '');
define('DB_DATABASE', 'penjualan');
define("SITE_KEY", 'PapkFti');
function getDB()
{ $dbhost=DB_SERVER;
 $dbuser=DB_USERNAME;
 $dbpass=DB_PASSWORD;
 $dbname=DB_DATABASE;
 $dbConnection = new PDO("mysql:host=$dbhost;dbname=$dbname", $dbuser, $dbpass);
 $dbConnection->exec("set names utf8");
 $dbConnection->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);
 return $dbConnection;
}
/* DATABASE CONFIGURATION END */
/* API key encryption */
function apiToken($session_uid)
{
$key=md5(SITE_KEY.$session_uid);
return hash('sha256', $key);
}
```

Selanjutnya buat file `index.php`. fungsinya adalah sebagai `web service` untuk menangani `request data` `get` dan `post` dari aplikasi ionic yang akan kita buat.

```php
<?php
header("Access-Control-Allow-Origin: *");
header('Access-Control-Allow-Methods: GET, PUT, POST, DELETE, OPTIONS');
header('Access-Control-Allow-Headers: Content-Type, Content-Range, Content-Disposition, Content-Description');
header('Content-Type: application/json');

require 'vendor/autoload.php';
require 'config.php';
$app = new Slim\App();

$app->get('/Data_Barang', 'Data_Barang');
$app->post('/Input_Barang', 'Input_Barang');
$app->post('/Get_Barang_Edit', 'Get_Barang_Edit');
$app->post('/Edit_Barang', 'Edit_Barang');
$app->post('/Delete_Barang', 'Delete_Barang');
$app->run();

//request semua data yang berada pada tabel barang
function Data_Barang($request, $response)
{
    $data = $request->getParsedBody();
    //$login=$data['login'];
    //$token=$data['token'];
    //$systemToken=apiToken($login);
    try {
        //if($systemToken == $token){
        $Data_Barang = '';
        $db = getDB();
        $sql = "SELECT * FROM barang order by id_barang desc";
        $stmt = $db->prepare($sql);
        $stmt->execute();
        $Data_Barang = $stmt->fetchAll(PDO::FETCH_OBJ);
        $db = null;
        if ($Data_Barang)
            echo '{"Data_Barang": ' . json_encode($Data_Barang) . '}';
        else
            echo '{"Data_Barang": ""}';
        //} else{
        //    echo '{"error":{"text":"No access"}}';
        //}
    } catch (PDOException $e) {
        echo '{"error":{"text":' . $e->getMessage() . '}}';
    }
}

//POST data barang untuk selanjutnya akan di simpan di tabel barang
function Input_Barang($request, $response)
{

    $data = $request->getParsedBody();
    $nama_barang = $data['nama_barang'];
    $qty = $data['qty'];
    $harga = $data['harga'];
    //$login=$data['login'];
    //$token=$data['token'];
    //$systemToken=apiToken($login);
    try {
        //if($systemToken == $token){
        $db = getDB();
        $sql = "INSERT INTO barang(nama_barang, qty, harga) VALUES(:nama_barang ,:qty, :harga)";
        $stmt = $db->prepare($sql);
        $stmt->bindParam("nama_barang", $nama_barang, PDO::PARAM_STR);
        $stmt->bindParam("qty", $qty, PDO::PARAM_STR);
        $stmt->bindParam("harga", $harga, PDO::PARAM_STR);
        $stmt->execute();
        $db = null;
        if ($stmt)
            echo '{"Input_Barang": "input success"}';
        else
            echo '{"Input_Barang": "input error"}';
        //} else{
        //    echo '{"error":{"text":"No access"}}';
        //}
    } catch (PDOException $e) {
        echo '{"error":{"text":' . $e->getMessage() . '}}';
    }
}

//request data yang berada pada tabel barang berdasarkan id_barang
function Get_Barang_Edit($request, $response)
{
    $data = $request->getParsedBody();
    $id_barang = $data['id_barang'];
    //$login=$data['login'];
    //$token=$data['token'];
    //$systemToken=apiToken($login);
    try {
        //if($systemToken == $token){
        $Get_Barang_Edit = '';
        $db = getDB();
        $sql = "SELECT * FROM barang WHERE id_barang=:id_barang";
        $stmt = $db->prepare($sql);
        $stmt->bindParam("id_barang", $id_barang, PDO::PARAM_STR);
        $stmt->execute();
        $Get_Barang_Edit = $stmt->fetchAll(PDO::FETCH_OBJ);
        $db = null;
        if ($Get_Barang_Edit)
            echo '{"Get_Barang_Edit": ' . json_encode($Get_Barang_Edit) . '}';
        else
            echo '{"Get_Barang_Edit": ""}';
        //} else{
        //    echo '{"error":{"text":"No access"}}';
        //}
    } catch (PDOException $e) {
        echo '{"error":{"text":' . $e->getMessage() . '}}';
    }
}

//POST data barang ubah data berdasarkan id_barang
function Edit_Barang($request, $response)
{
    $data = $request->getParsedBody();
    $id_barang = $data['id_barang'];
    $nama_barang = $data['nama_barang'];
    $qty = $data['qty'];
    $harga = $data['harga'];
    //$login=$data['login'];
    //$token=$data['token'];
    //$systemToken=apiToken($login);
    try {
        //if($systemToken == $token){
        $db = getDB();
        $sql = "UPDATE barang SET nama_barang=:nama_barang, qty=:qty, harga=:harga WHERE id_barang=:id_barang";
        $stmt = $db->prepare($sql);
        $stmt->bindParam("id_barang", $id_barang, PDO::PARAM_STR);
        $stmt->bindParam("nama_barang", $nama_barang, PDO::PARAM_STR);
        $stmt->bindParam("qty", $qty, PDO::PARAM_STR);
        $stmt->bindParam("harga", $harga, PDO::PARAM_STR);
        $stmt->execute();
        $db = null;
        if ($stmt)
            echo '{"Edit_Barang": "Edit success"}';
        else
            echo '{"Edit_Barang": "Edit error"}';
        //} else{
        //    echo '{"error":{"text":"No access"}}';
        //}
    } catch (PDOException $e) {
        echo '{"error":{"text":' . $e->getMessage() . '}}';
    }
}

//Untuk menghapus data barang berdasarkan id_barang
function Delete_Barang($request, $response)
{
    $data = $request->getParsedBody();
    $id_barang = $data['id_barang'];
    //$login=$data['login'];
    //$token=$data['token'];
    //$systemToken=apiToken($login);
    try {
        //if($systemToken == $token){
        $db = getDB();
        $sql = "DELETE FROM barang WHERE id_barang=:id_barang";
        $stmt = $db->prepare($sql);
        $stmt->bindParam("id_barang", $id_barang, PDO::PARAM_STR);
        $stmt->execute();
        $db = null;
        if ($stmt)
            echo '{"Delete_Barang": "Delete success"}';
        else
            echo '{"Delete_Barang": "Delete error"}';
        //} else{
        //    echo '{"error":{"text":"No access"}}';
        //}
    } catch (PDOException $e) {
        echo '{"error":{"text":' . $e->getMessage() . '}}';
    }
}
```

Terakhir buat file

```apache
.htaccess
```

Masukan syntax berikut di dalam file .htaccess

```apache
RewriteEngine On
RewriteCond %{REQUEST_FILENAME} !-f
RewriteCond %{REQUEST_FILENAME} !-d
RewriteRule ^ index.php [QSA,L]
```

`.htaccess` adalah file text sederhana dalam format ASCII. Pada umumnya kita bisa menggunakan file .htaccess untuk merubah beberapa konfigurasi untuk mengarahkan pada web server Apache. File .htaccess dapat ditaruh dimapun pada folder website Anda.

Disini saya tidak akan menerangkan lebih detail tentang penggunaan .htaccess ya. 😊

Namun jika nanti temen-temen jadi pengelola web server dan di dalamnya mengelola berbagai web app, wajib mempelajari lebih dalam tentang penggunaan `.htaccess.

> Intinya banyak hal yang dapat dilakukan dengan `.htaccess` 👨‍💻

Ok!, berikut struktur file dan folder setelah installasi slim framework dan menambahkan 3 file konfigurasi didalam direktori:

```php
1. config.php
2. index.php
3. .htaccess
```

![dir](images/Screen%20Shot%202021-11-29%20at%2021.22.17.png)

## Step 4: Test Web Service

Untuk mencoba menjalankan `web servicenya`, isikan data dahulu pada tabel `barang`. Coba scroll lagi ke atas barangkali kelewat untuk mengisikan data nya. 😁

Kemudian ketikan alamat berikut di browser:

```link
http://localhost/webservice-ionic/Data_Barang
```

Pastikan service apache dan MySQL dalam keadaan start ya.

Jika tidak ada PRO-BLEM `Data barang` akan di tampilkan seperti screenshoot di bawah ini:

![ss](images/Nov-29-2021%2021-42-21.gif)

Sampai disini kita tela berhasil membuat `Backend` `Web Service` untuk aplikasi ionic.

Untuk materi selanjutnya kita akan fokus di `frontend` nya menggunakan framework ionic.

## Referensi

1. [Slimframework](https://www.slimframework.com)
2. [doc/v3](https://www.slimframework.com/docs/v3/)
3. [Composer/doc](https://getcomposer.org/doc/00-intro.md#installation-windows)
