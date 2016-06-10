Untuk instalasi JON 3.3 di Windows:

  - Jalankan instalasi oleh user Administrator. Buka CMD.exe dengan cara "run as Administator"
  - Pastikan Java yang digunakan adalah versi 1.7 
  - Pastikan dulu JBOSS_HOME tidak diset di system environment sebelum melakukan instalasi.
  - Gunakan direktori instalasi yang tidak terlalu dalam atau memiliki nama yang panjang, sehingga **PATH dari direktori instalasi tidak lebih dari 19 karakter**. Cukup instal di direktori misalnya `C:\jboss\JON_3.3`
  - Pastikan hostname tidak me-resolve ke loopback IP address (127.0.0.1), gunakan perintah `hostname` dari CMD.exe untuk mengetahui nama host, kemudian coba ping dengan perintah `ping -4 <hostname>`. Jika output ping terlihat reply dari IP address 127.0.0.1 maka ubah dengan mengedit file `C:\System32\drivers\etc\hosts` masukan entry
      
      ```
      <ip_address>  <hostname>
      ```
      
    dimana `ip_address` adalah IP localhost yang bukan loopback interface.
      
Baca juga:
  - [Kebutuhan hardware minimum](https://access.redhat.com/documentation/en-US/Red_Hat_JBoss_Operations_Network/3.3/html/Installation_Guide/Hardware_Minimums.html)
  - [Default port yang digunakan JON 3.3](https://access.redhat.com/documentation/en-US/Red_Hat_JBoss_Operations_Network/3.3/html/Installation_Guide/sect-Preparing_for_Installation_on_nix_Systems.html#Configuring_Ports)
  - [Konfigurasi hardware/software yang didukung](https://access.redhat.com/articles/112523)

Untuk detail proses instalasi, silakan merujuk ke dokumentasi

[RED HAT JBOSS OPERATIONS NETWORK 3.3 INSTALLATION GUIDE](https://access.redhat.com/documentation/en-US/Red_Hat_JBoss_Operations_Network/3.3/html/Installation_Guide/index.html)
