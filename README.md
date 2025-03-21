# Adpro-Module6

# Module 6: Concurrency

## MILESTONE 1
Single-threaded web server bekerja dengan prinsip request-response menggunakan dua protokol utama, yaitu Transmission Control Protocol (TCP) dan Hypertext Transfer Protocol (HTTP). TCP bertanggung jawab atas perpindahan data antar server
tanpa menentukan isinya, sementara HTTP menentukan konten request dan response yang dikirim melalui TCP. Server awalnya hanya menerima koneksi dari browser menggunakan ``TcpListener`` pada alamat 127.0.0.1:7878 dan mencetak "Connection
established!" setiap kali koneksi diterima. Fungsi ``bind()`` menghubungkan ``TcpListener`` dengan alamat dan port
tertentu, sedangkan ``unwrap()`` mengekstrak nilai dari Result atau Option, menyebabkan panic jika terjadi error. Metode
``incoming()`` menangani koneksi masuk sebagai iterator yang menghasilkan ``Result<TcpStream, Error>``, memungkinkan
server menerima dan menangani koneksi secara berurutan. Untuk menangani request dari browser, digunakan metode
``handle_connection()``, yang pada akhirnya mencetak HTTP request ke konsol. Data permintaan dibaca menggunakan
BufReader yang membaca stream dari ``TcpStream`` melalui metode ``lines()``, sedangkan variabel ``http_request``
mengumpulkan baris-baris request HTTP yang diterima dari browser.

## MILESTONE 2
![MILESTONE 2 screen capture](/assets/image1.png)

## MILESTONE 3
Refaktor yang saya tambahkan adalah menggunakan If else untuk memuat halaman berdasarkan status respons.
![MILESTONE 3 screen capture](/assets/image2.png)

## MILESTONE 4
Fungsi thread::sleep digunakan untuk menunda eksekusi program dalam jangka waktu tertentu. Dengan menggunakan match, kita dapat memeriksa baris request yang diterima dari klien. Jika request yang diterima adalah "GET /sleep HTTP/1.1", maka server akan berhenti (sleep) selama 10 detik sebelum mengirimkan respons, mensimulasikan keterlambatan yang dapat memengaruhi permintaan lain ke server. Untuk menguji efeknya, kita bisa membuka dua jendela browser dan membandingkan respons dari endpoint / dan /sleep. Jika kita mengakses / setelah sebelumnya memuat /sleep, akan terlihat bahwa halaman / harus menunggu hingga proses sleep selama 10 detik selesai sebelum diproses.

## MILESTONE 5
Dalam implementasi multithreaded, ThreadPool diperbarui untuk menyimpan vektor berisi beberapa Worker, di mana setiap Worker memiliki ID unik dan sebuah thread yang awalnya diinisialisasi dengan closure kosong. Saat ThreadPool dibuat, sebuah channel juga dibentuk, di mana sender disimpan dalam ThreadPool, sementara receiver didistribusikan ke setiap Worker. Ketika execute dipanggil, closure yang diterima dikirim melalui sender channel agar dapat dieksekusi oleh salah satu thread yang tersedia. Setiap Worker terus mengambil tugas dari receiver channel dan menjalankannya dalam loop, menggunakan mutex untuk mencegah race condition. Dengan pendekatan ini, ThreadPool dapat menangani banyak tugas secara concurrent, menjaga jumlah thread tetap optimal untuk mengurangi overhead, serta memastikan pengiriman tugas antar-thread berlangsung dengan aman melalui channel.