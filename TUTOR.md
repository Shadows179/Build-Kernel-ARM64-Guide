[Tutorial Dasar] Cara Membangun Kernel ARM64 Kustom #949
Coconutat memulai percakapan ini di Show and Tell

[Tutorial Dasar] Cara Membangun Kernel ARM64 Kustom #949

@Coconutat
Coconutat
pada 13 Sep 2023 ·

[Tutorial Dasar] Cara Membangun Kernel ARM64 Kustom

Pertama, ini adalah narasi sederhana. Saya adalah pengguna perangkat Android amatir, jadi apa yang dibahas di sini adalah yang paling dasar. Teman-teman yang berpengalaman bisa melewatinya.

Kedua, saya berharap dapat menyederhanakan tutorial ini sebanyak mungkin untuk membantu pemula melewati periode percobaan dan kesalahan.

Karena sebagian besar perangkat Android modern berbasis arsitektur ARM64, panduan ini dirancang khusus untuk perangkat ARM64.

Jadi, mari kita mulai！

1. Persiapan Dasar:
PC dengan distribusi Linux (misalnya: Ubuntu, Deepin, Arch, Debian) atau Gunakan Mesin Virtual

Konfigurasi hardware yang direkomendasikan:

RAM: 8 / 16 GB
CPU: Minimal 4 Cores x86_64 Intel Core atau AMD Ryzen Processor
Hard disk: 250GB / 500 GB SSD atau HDD (SSD lebih direkomendasikan untuk mengurangi waktu kompilasi secara signifikan.)
Distribusi Linux yang direkomendasikan: Ubuntu 18.04 / 20.04 (Mengurangi waktu konfigurasi sistem untuk kompilasi kernel.)
Memiliki ketekunan

Pemahaman dasar tentang perintah Linux

Saya nyatakan di muka bahwa saya tidak akan mengajarkan cara menginstal distribusi Linux atau mesin virtual. Karena ini adalah tutorial tentang kompilasi kernel,
dan instalasi sistem operasi mudah ditemukan di tutorial online.

2. Instalasi Lingkungan Ketergantungan Dasar:
Saya akan menggunakan Ubuntu 20.04 sebagai contoh:

Buka terminal Anda dan gunakan perintah berikut untuk menginstal ketergantungan kompilasi dasar:

sudo apt install bc bison build-essential ccache curl flex g++-multilib gcc-multilib git git-lfs gnupg gperf imagemagick lib32ncurses5-dev lib32readline-dev lib32z1-dev libelf-dev
liblz4-tool libncurses5 libncurses5-dev libsdl1.2-dev libssl-dev libxml2 libxml2-utils lzop pngcrush rsync schedtool squashfs-tools xsltproc zip zlib1g-dev adb fastboot -y


Jika sistem Anda kekurangan lingkungan ketergantungan ini, kompilasi akan melaporkan kesalahan. 
Tentu saja, daftar di atas tidak menjamin 100% bahwa Anda tidak kekurangan lingkungan ketergantungan sama sekali. 
Anda perlu melakukan penyesuaian sesuai dengan kernel dan sistem yang ingin Anda kompilasi. Misalnya, kernel perangkat awal Huawei menggunakan Python 2, bukan Python 3. 
Jika Anda menggunakan Python 3, akan menyebabkan kesalahan kompilasi.

3. Pilih Kernel yang Tepat untuk Perangkat
Pertama, Anda perlu memahami versi Android dan versi kernel Android perangkat Anda. Dengan cara ini, Anda dapat memperoleh kernel yang benar dari kode sumber terbuka yang dipublikasikan oleh produsen.
Menurut persyaratan Google, perangkat dapat menerima setidaknya tiga pembaruan versi Android utama. Selama periode ini, produsen mungkin memodifikasi kernel. 
Jadi, Anda perlu mengetahui informasi ini untuk mendapatkan kernel yang benar.

Contoh perangkat:

Perangkat 1:

Nama Perangkat: Xiaomi 8
Kode Pengembangan: dipper
Versi Android: 10 (Android Q)
Versi Kernel: 4.9.186
Versi Sistem: MIUI 12.5.x
Dengan informasi ini, Anda dapat mengunjungi repositori kernel sumber terbuka Xiaomi untuk mengunduh kernel yang sesuai.

Perangkat 2:

Nama Perangkat: Huawei P10
Kode Pengembangan: Victoria
Versi Android: 9 (Android P)
Versi Kernel: 4.9.111
Versi Sistem: EMUI 9.0.1.xxx
Dengan informasi ini, Anda dapat mengunjungi halaman sumber terbuka Huawei untuk mengunduh kernel yang sesuai.
4. Persiapan Kompilasi
Pertama, Anda perlu memilih cross-compiler yang sesuai.

Apa itu cross-compiler? Lihat: Cross compiler

Secara sederhana, komputer Anda mungkin memiliki arsitektur x86, sementara ponsel Anda memiliki arsitektur ARM64. File biner untuk arsitektur yang berbeda tidak dapat dipertukarkan. 
Anda perlu menghasilkan file biner yang sesuai untuk masing-masing platform.

Memilih cross-compiler yang tepat:
Sebagian besar smartphone saat ini menggunakan compiler Clang. Namun, beberapa ponsel lama mungkin masih menggunakan compiler GCC. 
Oleh karena itu, Anda perlu mencari cross-compiler yang sesuai dengan situasi Anda.

Beberapa cross-compiler yang bisa dipertimbangkan:

Cross-Compiler GCC Google:
Google GCC: aarch64-linux-android-4.9
Clang Google:
Google Clang: Google Clang linux-x86
Cross-Compiler Pihak Ketiga yang Direkomendasikan:
Proton-Clang: Dikembangkan oleh kdrag0n
Linaro GCC 4.9-2017.01: Alternatif yang baik untuk Google GCC
5. Mulai Kompilasi
Di folder kosong yang sesuai, ekstrak kernel Anda, dan di folder kosong lain, ekstrak cross-compiler Anda. Kemudian, buka terminal di direktori root kode sumber.

[*] Jika menggunakan GCC:

- Setel variabel PATH lingkungan:
export PATH=$PATH:XXXXXX/bin

- Deklarasikan nama cross-compiler:
export CROSS_COMPILE=aarch64-linux-gnu-

- Deklarasikan arsitektur kernel yang ingin Anda kompilasi:
export ARCH=arm64

- Deklarasikan defconfig untuk kernel yang ingin Anda kompilasi:
make ARCH=arm64 O=out XXXXX_defconfig

- Mulai kompilasi:
make ARCH=arm64 O=out -jX 2>&1 | tee kernel_log.log

- Tunggu hasil kompilasi. Ada dua kemungkinan hasil: berhasil atau gagal. Memperbaiki kesalahan di luar lingkup tutorial ini.


[*] Jika menggunakan Clang:

Setel variabel PATH lingkungan:
export PATH=$PATH:XXXXXX/bin

- Deklarasikan nama cross-compiler dan variabel terkait:
export CC=clang
export CLANG_TRIPLE=aarch64-linux-gnu-
export CROSS_COMPILE=aarch64-linux-gnu-
export CROSS_COMPILE_ARM32=arm-linux-gnueabi-

- Deklarasikan arsitektur kernel yang ingin Anda kompilasi:
export ARCH=arm64

- Deklarasikan defconfig untuk kernel yang ingin Anda kompilasi:
make ARCH=arm64 O=out CC=clang XXXXX_defconfig

- Mulai kompilasi:
make ARCH=arm64 O=out CC=clang -jX 2>&1 | tee kernel_log.log

Tunggu hasil kompilasi, dan memperbaiki kesalahan tidak dicakup dalam tutorial ini.

6. Penutup
Kami telah mencapai akhir panduan ini. Kompilasi kernel pada dasarnya melibatkan beberapa langkah ini. 
Ini adalah percobaan pertama saya membuat tutorial komprehensif, dan saya dengan tulus menyambut umpan balik dan saran Anda. 
Jika Anda memiliki solusi yang lebih baik, silakan bagikan di forum diskusi. Jika ada kesalahan tata bahasa atau deskripsi, harap beri tahu saya untuk diperbaiki.
