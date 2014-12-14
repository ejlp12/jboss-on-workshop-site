
1. Download JBoss Operation Network (JON) dari website [Red Hat product download]. Yang akan kita gunakan pada workshop 
   ini adalah JON versi paling akhir yang ada saat ini (saat artikel ini dibuat) yaitu __versi 3.3__
   
   ![Halaman download JON][Halaman download JON]
   
   Selain itu, dalam workshop ini kita juga membutuhkan plugin untuk monitoring JBoss EAP (JBoss AS). Kita bisa download 
   dari website yang sama. Dengan memilih produk "JBoss ON for EAP"
   
   ![Halaman download Plugin JON for EAP][Halaman download Plugin JON]

   Jadi paling tidak kita memiliki 2 file berikut:
   
     - `jon-server-3.3.0.GA.zip`
     - `jon-plugin-pack-eap-3.3.0.GA.zip`
   
   
2. Download juga dokumentasi JON terutama Installation Guide sebagai penuntun dalam Lab instalasi ini. 
   Dokumentasi bisa di-download disini:
        
        https://access.redhat.com/documentation/en-US/Red_Hat_JBoss_Operations_Network/
        
3. Penting untuk dibaca sebelum instalasi adalah kebutuhan dasar dari JON, yaitu supported configuration terutama hardware
   spec, tipe dan versi OS, database, JDK/JRE. Supported configuration, bisa dilihat di link berikut: 
        
        https://access.redhat.com/articles/112523
   
   Di Lab ini kita akan menggunakan:
        * Linux (RHEL v6.5) atau Windows 8, 64bit
        * Database PostgreSQL v9.3.5 (terbaru saat tulisan ini dibuat)
        * Mesin minimal RAM 2GB, disk 10GB
        * JDK 1.7
        
4. Install JDK 1.6 atau 1.7 dan set variabel JAVA_HOME dan PATH

   Jika menggunakan Windows, misal kita instal JDK di direktori `C:\Java\jdk1.7.0_67` (Windows) atau di 
   `/usr/java/jdk1.7.0_67` (Linux)
   Set PATH variable pada system agar menggunakan JDK tersebut dengan cara berikut:
   
   Windows:
   
   ```
   setx JAVA_HOME "C:\Java\jdk1.7.0_67"
   setx PATH %JAVA_HOME%\bin;%PATH%
   ```
    
   Linux:
    
   ```bash
   export JAVA_HOME=/usr/java/jdk1.7.0_67
   export PATH=$PATH:$JAVA_HOME/bin
   ```
    
   Kemudian tes dengan perintah berikut `echo %PATH%` (Windows) atau `echo $PATH` (Linux)
   Pastikan direktori `C:\Java\jdk1.7.0_67\bin` atau `/usr/java/jdk1.7.0_67` ada di output hasil perintah diatas. 
   Kemudian pastikan juga dengan perintah `java -version` anda tidak mendapatkan error tapi menghasilkan output seperti ini:
    
   ```
   java version "1.7.0_67"
   Java(TM) SE Runtime Environment (build 1.7.0_67-b01)
   Java HotSpot(TM) 64-Bit Server VM (build 24.65-b04, mixed mode)
   ```
    
5. Instal PostgreSQL
   Download PostgreSQL. Saya menggunakan PostgreSQL versi EnterpriseDB yang saya download dari sini:

      http://www.enterprisedb.com/products-services-training/pgdownload

   Saya menggunakan versi paling baru (saat artikel ini dibuat) yaitu __versi 9.3.5__
   Untuk Linux yang digunakan adalah file installer dengan nama `postgresql-9.3.5-3-linux-x64.run` sedangkan jika menggunakan
   Windows filenya adalah `postgresql-9.3.5-3-windows-x64.exe`
   
   Jalankan dan tes PostgreSQL dengan memikuti tutorial berikut:

         http://www.enterprisedb.com/resources-community/tutorials-quickstarts/windows/getting-started-postgres-plus-tutorial-windows

   Ubah password OS dari user postgres dan juga password dari user postgres untuk akses ke database. Dari user `root` 
   ketikan perintah berikut
    
    Linux:
    ```sql
    su - postgres
    psql
    postgres=# ALTER USER postgres PASSWORD 'password'; ALTER ROLE
    postgres=#
    ```
   
   Tambahkan user rhqadmin ke database, jangan ubah passwordnya. Jika anda mengubah passwordnya, sebelum menjalankan 
   JON Server, nanti anda perlu mengubah password di file konfigurasi JON (`rhq-server.properties`)

    ```sql
    postgres=# CREATE USER rhqadmin PASSWORD 'rhqadmin'; CREATE ROLE 
    ```
    
    Kemudian buat database untuk untuk JON

    ```sql
    postgres=# CREATE DATABASE rhq OWNER rhqadmin; CREATE DATABASE
    ```
    
    Restart PostgreSQL database

   Sekarang kita instal JON, ekstrak file jon-server-3.3.0.GA.zip di sebuah direktori, misal di `C:\JBoss\jon` (Windows) 
   atau di `/home/jboss/jon` (Linux). Jadi direktori instalasi JON ada di `C:\JBoss\jon-server-3.3.0.GA\` atau di 
   `/home/jboss/jon/jon-server-3.3.0.GA\`. Berikut adalah struktur direktori dari JON:
    
   
   ```
    - EULA 
    - INSTALL_README.txt
    - LICENSE
    - UPGRADE_README.txt
    - alert-scripts/
    - bin/
    - etc/
    - jbossas/
    - logs/
    - modules/
    - plugins/
    - rhq-storage/
   ```
   
   Masuk ke direktori instalasi tersebut, `<JON_INSTALL_DIR>\bin` kemudian jalankan perintah

    ``
    cd bin
    rhqctl.bat install --start
    ``
    
    Perintah tersebut akan melakukan instalasi JON dan setelahnya akan langsung menjalankannya (start). Perlu diingat password yang anda masukan saat ditanya untuk memasukan password. Password tersebut akan kita gunakan untuk mengakses web interface. Perlu diperhatikan juga pada saat ditanya IP address binding, masukan 0.0.0.0 artinya port service dari JON akan di binding ke semua network interfaces.

    Berikut output/input hasil perintah tersebut:

    ```	
    15:33:45,768 INFO  [org.jboss.modules] JBoss Modules version 1.3.3.Final-redhat-1
     
    The [rhq.autoinstall.server.admin.password] property is required but not set in [rhq-server.properties].
    Do you want to set [rhq.autoinstall.server.admin.password] value now?
    yes|no: yes
    rhq.autoinstall.server.admin.password (enter as plain text): <PASSWORD>
    Confirm:
    rhq.autoinstall.server.admin.password (enter as plain text): <PASSWORD>     
     
    The [jboss.bind.address] property is required but not set in [rhq-server.properties].
    Do you want to set [jboss.bind.address] value now?
    yes|no: yes
    jboss.bind.address: 0.0.0.0
    Is [0.0.0.0] correct?
    yes|no: yes
    15:34:11,135 INFO  [org.rhq.server.control.command.Install] Preparing to install RHQ storage node.
    Starting RHQ Storage Installer ...
    15:34:11,816 INFO  [org.jboss.modules] JBoss Modules version 1.3.3.Final-redhat-1
    15:34:11,943 INFO  [org.rhq.storage.installer.StorageInstaller] Running RHQ Storage Node installer...
    15:34:11,992 INFO  [org.rhq.storage.installer.StorageInstaller] Checking perms for ../../../rhq-data/saved_caches
    15:34:11,993 INFO  [org.rhq.storage.installer.StorageInstaller] Checking perms for ../../../rhq-data/commit_log
    15:34:11,993 INFO  [org.rhq.storage.installer.StorageInstaller] Checking perms for ../../../rhq-data/data
    15:34:11,997 INFO  [org.rhq.cassandra.Deployer] Unzipping storage node to /Users/eariobow/Documents/03_RH_JBOSS_INSTALLER/JON_3.3.0/jon-server-3.3.0.GA/rhq-storage
    15:34:12,642 INFO  [org.rhq.cassandra.Deployer] Applying configuration changes to /Users/eariobow/Documents/03_RH_JBOSS_INSTALLER/JON_3.3.0/jon-server-3.3.0.GA/rhq-storage/conf/cassandra.yaml
    15:34:12,703 INFO  [org.rhq.cassandra.Deployer] Applying configuration changes to /Users/eariobow/Documents/03_RH_JBOSS_INSTALLER/JON_3.3.0/jon-server-3.3.0.GA/rhq-storage/conf/log4j-server.properties
    15:34:12,707 INFO  [org.rhq.cassandra.Deployer] Applying configuration changes to /Users/eariobow/Documents/03_RH_JBOSS_INSTALLER/JON_3.3.0/jon-server-3.3.0.GA/rhq-storage/conf/cassandra-jvm.properties
    15:34:12,717 INFO  [org.rhq.cassandra.Deployer] Updating file permissions in /Users/eariobow/Documents/03_RH_JBOSS_INSTALLER/JON_3.3.0/jon-server-3.3.0.GA/rhq-storage/bin
    15:34:12,734 INFO  [org.rhq.storage.installer.StorageInstaller] Updating rhq-server.properties...
    15:34:12,756 INFO  [org.rhq.storage.installer.StorageInstaller] Installation of the storage node is complete
    15:34:13,099 INFO  [org.rhq.server.control.command.Install] The storage node installer has finished with an exit value of 0
     INFO 15:34:14,035 Logging initialized
     INFO 15:34:14,046 JVM vendor/version: Java HotSpot(TM) 64-Bit Server VM/1.6.0_65
     INFO 15:34:14,047 Heap size: 523501568/523501568
    ```

    Perintah tersebut juga akan menjalankan JON Agent, kita bisa lihat dibagian akhir output dari perintah tersebut


    	
    ```
    15:35:45,672 INFO  [org.rhq.server.control.command.Install] Installing RHQ agent
    ======================================
    ANT target [(default)]
    Wed Dec 10 15:35:46 WIT 2014
    ======================================
    [header-for-install] [echo]
    ===== RHQ AGENT INSTALL =====
    Installing Agent To: /Users/eariobow/Documents/03_RH_JBOSS_INSTALLER/JON_3.3.0
    Version: 4.12.0.JON330GA
    Build Number: e347f77
    Jar File: /Users/eariobow/Documents/03_RH_JBOSS_INSTALLER/JON_3.3.0/jon-server-3.3.0.GA/modules/org/rhq/server-startup/main/deployments/rhq.ear/rhq-downloads/rhq-agent/rhq-enterprise-agent-4.12.0.JON330GA.jar
    [install] [echo] Extract the agent distro zip from the agent update binary
    [install] [unjar] Expanding: /Users/eariobow/Documents/03_RH_JBOSS_INSTALLER/JON_3.3.0/jon-server-3.3.0.GA/modules/org/rhq/server-startup/main/deployments/rhq.ear/rhq-downloads/rhq-agent/rhq-enterprise-agent-4.12.0.JON330GA.jar into /Users/eariobow/Documents/03_RH_JBOSS_INSTALLER/JON_3.3.0/jon-server-3.3.0.GA
    [install] [echo] Unzip the agent distro into the new installation directory
    [install] [unzip] Expanding: /Users/eariobow/Documents/03_RH_JBOSS_INSTALLER/JON_3.3.0/jon-server-3.3.0.GA/rhq-enterprise-agent-4.12.0.JON330GA.zip into /Users/eariobow/Documents/03_RH_JBOSS_INSTALLER/JON_3.3.0
    [install] [echo] chmod +x on executables under /Users/eariobow/Documents/03_RH_JBOSS_INSTALLER/JON_3.3.0/rhq-agent
    [install] [echo] Remove the agent distro zip
    [install] [delete] Deleting: /Users/eariobow/Documents/03_RH_JBOSS_INSTALLER/JON_3.3.0/jon-server-3.3.0.GA/rhq-enterprise-agent-4.12.0.JON330GA.zip
    [install] [echo] DONE! Agent version 4.12.0.JON330GA (build number=e347f77) has been installed to /Users/eariobow/Documents/03_RH_JBOSS_INSTALLER/JON_3.3.0
    15:35:47,423 INFO  [org.rhq.server.control.command.Install] The agent installer finished running with exit value 0
    15:35:47,424 INFO  [org.rhq.server.control.command.Install] Configuring the RHQ agent with default configuration file: /Users/eariobow/Documents/03_RH_JBOSS_INSTALLER/JON_3.3.0/rhq-agent/conf/agent-configuration.xml
    15:35:47,510 INFO  [org.rhq.server.control.command.Install] Finished configuring the agent
    15:35:47,510 INFO  [org.rhq.server.control.command.Install] Starting RHQ agent...
    Starting RHQ Agent...
    RHQ Agent (pid 1753 ) is ✔running
    15:35:52,801 INFO  [org.rhq.server.control.command.Install] The agent has started up
    ```

    Akses web UI dengan menggunakan browser ke URL berikut: 
    
      [http://localhost:7080/](http://localhost:7080/)
    
    Masuk dengan user `rhqadmin` dan password sesuai yang anda masukan saat instalasi

        ![Halaman Login JON Web UI](http://4.bp.blogspot.com/-kjSazqOR5jk/VIgIxDG87ZI/AAAAAAAADQw/jUwfpJDI30o/s1600/Snap%2B2014-12-10%2Bat%2B15.47.25.png)

---
[Red Hat product download]: https://access.redhat.com/jbossnetwork/restricted/listSoftware.html?downloadType=distributions&product=em&version=3.3&productChanged=yes
[Halaman download JON]: http://1.bp.blogspot.com/-pTnPmcii27I/VIf9keceSjI/AAAAAAAADQQ/6rEbPZEBf-g/s1600/Snap%2B2014-12-10%2Bat%2B15.00.01.png
[Halaman download Plugin JON]: http://3.bp.blogspot.com/-wmmeqm4Gfc0/VIzVGWoYksI/AAAAAAAADSc/E_0_v1vQ62Q/s1600/Snap%2B2014-12-14%2Bat%2B06.56.01.png
[Gambar struktur direktori JON]: http://2.bp.blogspot.com/-UmesMA7mNjA/VIgDhTRe6II/AAAAAAAADQg/lCSHUYyN3Ms/s1600/Snap%2B2014-12-10%2Bat%2B15.25.22.png


