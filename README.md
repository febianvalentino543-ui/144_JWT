# Praktikum Pengembangan Web Service – Pertemuan 11
## Service Hashing Password & JWT pada REST API Komik
 
Dokumen ini adalah bukti praktikum pembuatan **service hashing (bcrypt)** dan **service JWT (jsonwebtoken)** pada REST API berbasis **Express.js + Sequelize**, beserta hasil pengujian endpoint menggunakan **Postman**.
 
---
 
## 1. Tujuan
 
1. Membuat service untuk **hashing password** agar password tidak tersimpan sebagai teks biasa di database.
2. Membuat service untuk **JWT (JSON Web Token)** agar pengguna yang sudah login dapat mengakses endpoint yang dilindungi.
3. Menguji endpoint **register, login, CRUD komik, dan get by id** menggunakan Postman.
---
 
## 2. Teknologi yang Digunakan
 
| Teknologi | Fungsi |
|---|---|
| Node.js & Express.js | Framework REST API |
| Sequelize | ORM untuk koneksi dan query database |
| bcrypt | Hashing password |
| jsonwebtoken | Membuat dan memverifikasi JWT |
| dotenv | Menyimpan konfigurasi rahasia di `.env` |
| Postman | Pengujian endpoint |
 
---
 
 
## 3. Alur Kerja Autentikasi
 
```
REGISTER : password  ->  hashPassword()  ->  simpan hash ke database
LOGIN    : password  ->  comparePassword() cocok?  ->  generateToken()  ->  kirim token
REQUEST  : header Authorization: Bearer <token>  ->  verifyToken()  ->  lanjut ke controller
```
 
---
 
## 4. Hasil Pengujian Postman
 
> Semua endpoint komik membutuhkan header:
> `Authorization: Bearer <token hasil login>`
 
### 4.1 Register
 
- **Method:** `POST`
- **URL:** `http://localhost:3000/api/register`
- **Body (JSON):**
```json
{
    "email": "ac@gmail.com",
    "password": "12345"
}
```
 
<img width="3000" height="1876" alt="image" src="https://github.com/user-attachments/assets/949b5fc6-e3e2-47e9-b7e6-697f31f0bb97" />
 
**Keterangan:** User berhasil dibuat. Password di database tersimpan dalam bentuk hash bcrypt, bukan teks asli.
 
### 4.2 Login
 
- **Method:** `POST`
- **URL:** `http://localhost:3000/api/login`
- **Body (JSON):**
```json
{
    "email": "ac@gmail.com",
    "password": "12345"
}
```
 
<img width="2998" height="1876" alt="image" src="https://github.com/user-attachments/assets/848afadb-e339-4e8e-ba31-a2dedf79e306" />

**Keterangan:** Server mengembalikan token JWT. Token ini digunakan pada header `Authorization` untuk endpoint berikutnya.
 
### 4.3 Create Komik (POST)
 
- **Method:** `POST`
- **URL:** `http://localhost:3000/api/komik`
- **Header:** `Authorization: Bearer <token>`
  
<img width="3000" height="1876" alt="image" src="https://github.com/user-attachments/assets/cf7211d6-7d7e-4e53-ae2b-e6c9706a818f" />

**Keterangan:** Data komik baru berhasil ditambahkan.
 
### 4.4 Get All Komik (GET)
 
- **Method:** `GET`
- **URL:** `http://localhost:3000/api/komik`
- **Header:** `Authorization: Bearer <token>`
  
<img width="3000" height="1876" alt="image" src="https://github.com/user-attachments/assets/34068b12-9df7-4edd-b85a-7c576650f49c" />

 
**Keterangan:** Seluruh data komik berhasil ditampilkan.
 
### 4.5 Get Komik by ID
 
- **Method:** `GET`
- **URL:** `http://localhost:3000/api/komik/4`
- **Header:** `Authorization: Bearer <token>`
  
<img width="3000" height="1876" alt="image" src="https://github.com/user-attachments/assets/51a7f373-4cbb-463e-ac10-a41e57fd3846" />

 
**Keterangan:** Data komik dengan `id` yang diminta berhasil ditampilkan.
 
### 4.6 Update Komik (PUT)
 
- **Method:** `PUT`
- **URL:** `http://localhost:3000/api/komik/4`
- **Header:** `Authorization: Bearer <token>`
  
<img width="3000" height="1876" alt="image" src="https://github.com/user-attachments/assets/5596e752-16e1-4c23-bb06-333937b79764" />

 
**Keterangan:** Data komik berhasil diperbarui.
 
### 4.7 Delete Komik (DELETE)
 
- **Method:** `DELETE`
- **URL:** `http://localhost:3000/api/komik/4`
- **Header:** `Authorization: Bearer <token>`
  
<img width="3000" height="1876" alt="image" src="https://github.com/user-attachments/assets/584ed4ff-e688-4c63-83ad-24448fa672f7" />

 
**Keterangan:** Data komik berhasil dihapus.
 
---
 
## 5. Kesimpulan
 
1. Password disimpan dalam bentuk **hash bcrypt**, sehingga aman walaupun database bocor.
2. **Service hashing** dan **service JWT** dipisahkan dalam folder `services` sehingga kode lebih rapi dan mudah dipakai ulang.
3. **JWT** membuat endpoint komik hanya bisa diakses oleh pengguna yang sudah login.
4. Seluruh endpoint (register, login, CRUD, get by id) berhasil diuji menggunakan Postman.
---
 
