Enterprise Network Infrastructure: Hybrid Connectivity & Public Services 📋 Project Overview Proyek ini mensimulasikan infrastruktur jaringan perusahaan berskala menengah yang menghubungkan beberapa departemen (Kantor A & B) melalui jalur ISP. Fokus utama proyek ini adalah implementasi routing dinamis, keamanan akses internet melalui NAT, dan penyediaan layanan publik internal seperti Web dan DNS Server.

🛠️ Technical Specifications 
Inter-VLAN Routing: Menggunakan Multilayer Switch untuk memisahkan trafik departemen HR, Admin, dan Manager.
Dynamic Routing: Implementasi OSPF Area 0 untuk pertukaran rute otomatis antar segmen jaringan.
Address Translation: Konfigurasi PAT (Port Address Translation) agar perangkat lokal dapat mengakses internet menggunakan satu IP publik.
Network Services: Deployment Web Server dan DNS Server (reinssdev.com) pada segmen jaringan terpisah di sisi ISP.
Access List Control: Implementasi Access Control List (ACL) untuk manajemen hak akses dari setiap user berdasarkan aturan VLAN sesuai devisi guna meningkatkan keamanan data perusahaan.

🔍 Troubleshooting Highlights (The "Solving" Process) Bagian ini menunjukkan kompetensi saya dalam mendiagnosis masalah jaringan yang kompleks:

Masalah: Kegagalan Distribusi DHCP pada VLAN.
Gejala: PC di segmen VLAN tidak mendapatkan IP address meskipun ip helper-address sudah dikonfigurasi. 
Analisis: Ditemukan bahwa Router Kantor tidak memiliki jalur balik (return path) ke network VLAN tersebut. 
Solusi: Mendaftarkan network VLAN ke dalam proses OSPF pada Multilayer Switch, sehingga Router Kantor memiliki tabel routing yang lengkap untuk mengirimkan paket DHCP Offer kembali ke klien.

Masalah: Kegagalan Manager melakukan ICMP terhadap HR dan Admin meskipun hak akses untuk Manager telah diberikan izin sepenuhnya.
Gejala: PC di segmen VLAN Manager melakukan test ping terhadap kedua segmen VLAN HR dan Admin memastikan bahwa Access Control List dapat berjalan dengan semestinya. Namun, hasilnya (Request Timeout) dikarenakan untuk melakukan ICMP harus memiliki 2 jalur yang saling terhubung.
Analisis: Ditemukan bahwa terdapat satu list yang harus dimasukkan.
Solusi: Menambahkan satu list tersebut ke dalam Access Control List, sehingga Manager bisa melakukan test ping. Namun, untuk HR dan Admin tidak bisa melakukan ping.

Masalah: Kegagalan Akses Web via IP (Request Timeout) 
Gejala: Ping ke IP server berhasil, namun layanan HTTP tidak dapat diakses melalui browser. 
Analisis: Konfigurasi Default Gateway pada Server bernilai 0.0.0.0, Interface Router ISP yang mengarah ke server belum memiliki alamat IP dan berstatus shutdown. 
Solusi: Mengaktifkan interface fisik pada Router ISP, memberikan IP 10.0.0.1, dan mensinkronisasikan Gateway pada sisi Server.

Masalah: Error "Invalid Input" saat Konfigurasi Port
Gejala: Perintah ip address ditolak pada interface Fa0/1/0.
Analisis: Port tersebut merupakan bagian dari modul switchport (Layer 2) yang tidak mendukung IP address secara langsung.
Solusi: Mengalihkan koneksi ke port GigabitEthernet (Layer 3 murni) yang mendukung konfigurasi IP address secara native.

🌐 Final Result Jaringan berhasil terintegrasi penuh. PC Client dapat mengakses http://reinssdev.com secara transparan melalui resolusi nama oleh DNS Server lokal yang dikonfigurasi pada segmen ISP, serta memberikan hak akses izin dari setiap segmen VLAN agar meningkatkan keamanan data perusahaan.
