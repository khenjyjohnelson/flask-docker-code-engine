<h1 align="center"> Python Flask API to Code Engine </h1>
<p align="center"> Deploy kontainer Docker yang berisi Flask API untuk load model Machine Learning berbasis Saved Model dari Keras dan TensorFlow ke layanan IBM Cloud Code Engine</p>

<div align="center">

<img src="https://img.shields.io/badge/react-%2320232a.svg?style=for-the-badge&logo=react&logoColor=%2361DAFB">
<img src="https://img.shields.io/badge/python-3670A0?style=for-the-badge&logo=python&logoColor=ffdd54">
<img src="https://img.shields.io/badge/Keras-%23D00000.svg?style=for-the-badge&logo=Keras&logoColor=white">
<img src="https://img.shields.io/badge/TensorFlow-%23FF6F00.svg?style=for-the-badge&logo=TensorFlow&logoColor=white">
<img src="https://img.shields.io/badge/flask-%23000.svg?style=for-the-badge&logo=flask&logoColor=white">
<img src="https://img.shields.io/badge/docker-%230db7ed.svg?style=for-the-badge&logo=docker&logoColor=white">

</div>

# A. Syarat

- Perangkat dengan sistem Windows 10/11 64-bit
- RAM minimal 4GB
- Penyimpanan tersisa minimal 50-70GB
- Aplikasi Postman (download [disini](https://www.postman.com/downloads/)) +- 130MB
- Docker Desktop (download [disini](https://www.docker.com/products/docker-desktop/)) +- 500MB
- Python 3.8+ 
- Akun Docker Hub (daftar [disini](https://hub.docker.com/signup))
- Akun IBM Cloud 


# B. Langkah - langkah
Langkah - langkah akan dibuat per bagian dan akan memiliki sub bagian.

## 1. Persiapan akun dan repositori DockerHub
- Pastikan akun DockerHub sudah dibuat : 
![alt text](readme-img/image.png)

- Klik bagian 'Repositories' lalu buat sebuah repositori kosong baru
![alt text](readme-img/image-1.png)

- Buat nama repositori sesuka hati, nama ini akan dibuat sama dengan nama Docker image nantinya, biarkan _visibility_ di 'Public'.
![alt text](readme-img/image-3.png)

- Repositori berhasil dibuat.
![alt text](readme-img/image-4.png)

- Instal lalu buka aplikasi Docker Desktop anda
![alt text](readme-img/image-8.png)
Ini merupakan tampilan awal dari aplikasi Docker Desktop.

- Pastikan Docker Machine sudah dalam status hijau dan 'running' (ada di pojok kiri bawah)
![alt text](readme-img/image-9.png)

- Silakan login ke akun DockerHub anda di aplikasi Docker Desktop. (Tombol putih pojok kanan atas)
![alt text](readme-img/image-10.png)

- Anda akan dibawa ke browser, dan ikuti saja step nya.
![alt text](readme-img/image-11.png)

- Berikut tampilan jika anda sudah login ke Aplikasi Docker
![alt text](readme-img/image-12.png)
Akun anda akan berada di pojok kanan atas. Lanjutkan ke **tahap 2.**


## 2. Pastikan Flask API anda telah bekerja dengan baik

- Buka project Flask API anda <br />
![alt text](readme-img/image-5.png)

Ini merupakan struktur folder untuk proyek contoh yang ada di repositori ini.

-  Buka terminal anda, dan jalankan proyek Flask API anda dengan 
```
python -m flask run
```
Atau
```
python3 -m flask run
```
Atau
```
py -m flask run
```
Atau
```
python app.py / python3 app.py / py app.py
```
![alt text](readme-img/image-6.png)

Jika berjalan dengan aman di alamat ```http://127.0.0.1:5000``` atau ```http://localhost:5000``` dengan port 5000 (yang merupakan default), maka anda bisa lanjut ke **tahap 3**. Jika belum, silakan perbaiki hingga aplikasi Flask anda berjalan dengan baik.

## 3. Pembuatan Docker Image

- Pergi ke Visual Studio Code dan buka projek Flask API anda, dan buatlah sebuah ``` Dockerfile ```
![alt text](readme-img/image-7.png)

Proyek ini akan menggunakan docker base image dari python3.8-slim-buster
```
FROM python:3.8-slim-buster
```

Untuk isi dari file ini, silakan sesuaikan dengan kebutuhan anda, terutama untuk pemasangan dependency yang ada pada ``` requirements.txt ```

Sisanya, tidak perlu diubah banyak, silakan ikuti format diatas saja.

- Kembali ke terminal, lalu buat Docker Image dengan perintah berikut :

```
docker build --tag flask-docker .
```

``` flask-docker ``` akan menjadi nama dari Docker Image anda.

- Setelahnya, tunggu proses pembuatan Docker Image, yang akan memakan waktu hingga 10-20 menit dan menggunakan ruang sebanyak 4-5 GB.
![alt text](image.png)

- Berikut jika pembuatan Docker Image sudah selesai.
![alt text](image-1.png)
- Cek image yang tersedia dengan perintah : 

```
docker images
```
![alt text](image-2.png)

```flask-docker``` adalah nama repositori (karena nama dari image itu tidak kita spesifik kan diawal pembuatan) dari Docker Image yang berhasil dibuat dari proyek Flask API sebelumnya. Sesuai dengan nama repositori yang sebelumnya dibuat di DockerHub.

- Jalankan Docker Image menggunakan port 5000 untuk browser dari sistem host (mode detached) dengan perintah berikut: 

```
docker run -d -p 5000:5000 flask-docker
```

```-d``` untuk mengaktifkan image dalam mode detached. <br />
```-p``` untuk menspesifikkan nomor port. Kita akan menggunakan port 5000 yang merupakan default dari Flask.
<br />
Berikut adalah ketika Docker Image sudah berjalan : 
![alt text](image-3.png)

Image yang sedang berjalan dapat di cek menggunakan command:
```
docker ps
```

- Cek apakah Flask API didalam kontainer Docker Image sudah bisa diakses atau belum di port 5000

- Melalui browser, buka ```http://localhost:5000```

![alt text](image-4.png)

Tulisan 'Hello World!' yang merupakan isi dari route '/' atau route utama pada Flask API sudah dapat ditampilkan.

- Melalui aplikasi Postman

![alt text](image-5.png)

Gunakan method **GET** lalu masukkan alamat ```http://localhost:5000``` untuk melihat hasil yang diberikan yaitu pada bagian 'body'.

- Mencoba kinerja model melalui aplikasi Postman

Gunakan method **POST** lalu masukkan alamat ```http://localhost:5000/predict``` , lalu pilih **Body** dan pilih 'raw' pada tab dibawah kolom input dan pilih JSON pada pilihan dropdown di tab tersebut.

Gunakan data ini (silakan diubah sesuka hati nilainya) pada 'Body' dan pilih JSON sebagai format: 

```
{
  "input": [
    {"Tavg": 27, "RH_avg": 89, "ss": 6, "ff_avg": 4},
    {"Tavg": 24, "RH_avg": 89, "ss": 4, "ff_avg": 7},
    {"Tavg": 29, "RH_avg": 78, "ss": 6, "ff_avg": 5},
    {"Tavg": 33, "RH_avg": 77, "ss": 8, "ff_avg": 3},
    {"Tavg": 33, "RH_avg": 76, "ss": 9, "ff_avg": 4},
    {"Tavg": 25, "RH_avg": 88, "ss": 5, "ff_avg": 6},
    {"Tavg": 29, "RH_avg": 76, "ss": 7, "ff_avg": 5},
    {"Tavg": 30, "RH_avg": 80, "ss": 6, "ff_avg": 6},
    {"Tavg": 34, "RH_avg": 73, "ss": 9, "ff_avg": 3},
    {"Tavg": 19, "RH_avg": 93, "ss": 4, "ff_avg": 7}
  ]
}
```

Berikut pada hasil predikasi pada 'body' akan ditampilkan hasil response dari model prediksi yang ada didalam Flask API yang ada didalam Docker Image. 

```
{
    "predictions_lstm": [
        [
            3.5333642959594727
        ]
    ],
    "predictions_lstm_attention": [
        [
            36.94091796875
        ]
    ]
}

```

![alt text](image-6.png)

- Pastikan proses diatas sudah berhasil lancar, jika sudah, matikan container Docker Image yang berjalan, pada kasus ini, untuk nama container yang dipakai adalah ```romantic_johnson``` (dapat di cek di kolom NAMES ketika menjalankan perintah ```docker ps```, **setiap membuat image, nama container yang dipakai akan berbeda setiap perangkat**) ini merupakan nama random yang dibuat secara otomatis ketika pembuatan docker image. Matikan container dengan perintah : 

```
docker stop romantic_johnson
```
Lalu cek status di 

```
docker ps
```
Jika sudah kosong maka container yang menjalankan Docker Image ```flask-docker``` telah nonaktif.

![alt text](image-7.png)

- Lakukan pemberian 'tag' pada Docker Image sebelum  melakukan push ke repositori

Berikan tag ke Docker Image dengan nama ```latest```, nama ini boleh diganti asal sesuai dengan yang di repositori.

```
docker tag flask-docker:latest arifian823/flask-docker:latest
```

Bagian ```arifian823/flask-docker:latest``` merupakan nama repositori dengan tag terbaru nya yaitu ```latest```


- Push Docker Image ke repositori yang sudah dibuat sebelumnya di Docker Hub dengan perintah berikut : 

```
docker push arifian823/flask-docker:latest
```

Setelah melakukan proses tag, proses push akan memakan waktu yang cukup lama, sekitar 15-30 menit, tergantung ukuran image, untuk kasus ini, ukuran image hampir mencapai 4 GB, maka kecepatan internet akan mempengaruhi waktu proses push ke repositori.

![alt text](image-8.png)

- Ketika proses push telah selesai, maka pada terminal akan seperti ini dan hasil push juga dapat diakses di repositori yang ada di Docker Hub.

![alt text](image-9.png)

- Pada Docker Hub, terdapat tag ```latest``` yang merupakan image yang telah di push.
![alt text](image-10.png)

## 4. Proses Deployment ke Code Engine

Soon.