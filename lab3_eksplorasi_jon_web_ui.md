# LAB3: Eksplorasi JON core Web GUI (Dashboard)

> Pada Lab ini kita akan sedikit lebih mengeksplorasi core GUI dari JON.

1. Login ke JBoss Operation Network's core GUI `http://<IP_JON_SERVER>:7080/coregui/`
2. Tampilan awal core GUI adalah Dashbord view seperti ini:


Explore web UI untuk

1. Memonitor item-item berikut:
    * Penggunaan memory
    * Penggunaan CPU
    * Penggunaan File System
    * Utilisasi network adapter
    * JVM memory pool, garbage collector, kita bisa coba melihat JVM yang menjalankan RHQ Agent
2. Mecoba membuat Alert
3. Lihat konfigurasi JON Server
    * Tambah user
    * Lihat system setting dan ubah setting "Enable Experimental Features" menjadi true
    * Lihat daftar Server/Agent plugin lalu "Scan for Update" dan "Update Plugin on Agent"
    
    
Selesai!

Catatan: Ada hidden page di RHQ yang mungkin berguna, silakan dieksplor `http://<IP_JON_SERVER>:7080/coregui/#Test`
