Pada LAB2 ini menggunakan asumsi anda menggunakan Linux/*nix/OS-X

1. Pastikan semua komponen proses JON/RHQ dalam kondisi mati. Check status tiap-tiap proses dari komponen JON
   
   ```sh
   cd <JON_INSTALL_DIR>/bin
   ./rhqctl status
   ```
2. Sekarang kita akan jalankan komponen JON/RHQ satu persatu dalam mode console dan __DEBUG__. Sehingga kita bisa
   melihat lebih detail apa yang terjadi pada setiap komponen. Komponen yang perlu dijalankan sesuai urutan adalah:
   
     1. Database PostgreSQL
     2. RHQ Storage Node
     3. RHQ Server
     4. RHQ Agent
   
   Untuk membuat proses jalan dalam mode debug kita perlu set environment variable `RHQ_CONTROL_DEBUG` bernilai `true`. 
   Jalan perintah berikut pada sebuah terminal/shell prompt untuk menjalankan __RHQ Storage Node__
   
   ```bash
   cd /home/jboss/jon-server-3.3.0.GA/bin
   export RHQ_CONTROL_DEBUG=true
   ./rhqctl console --storage
   ```
   
   Tunggu sampai output memperlihatkan output seperti ini:
  
   ```
   INFO 15:44:14,107 Startup completed! Now serving reads.
   INFO 15:44:14,355 Starting listening for CQL clients on MyMacBook-Pro.local/192.168.1.101:9142...
   INFO 15:44:14,378 Not starting RPC server as requested. Use JMX (StorageService->startRPCServer()) or nodetool (enablethrift) to start it
   ```
   
   >> Saat RHQ Storage Node dijalankan dalam mode console, perintah `rhqctl status` akan tetap memperlihatkan bahwa 
      RHQ Storage Node dalam keadaan mati (down)
   
3. Setelah RHQ Storage selesai dijalankan. Barulah jalankan __RHQ Server__ dengan perintah berikut di sebuah termial/shell 
   prompt baru:
   
   ```bash
   cd /home/jboss/jon-server-3.3.0.GA/bin
   export RHQ_CONTROL_DEBUG=true
   ./rhqctl console --server
   ```
   
   Tunggu sampai ditampilkan output seperti ini:
   
   ```
   16:21:33,579 INFO  [org.rhq.enterprise.server.core.StartupBean] (pool-6-thread-1) Creating timer to begin scanning for plugins...
   16:21:33,619 INFO  [org.rhq.enterprise.server.core.StartupBean] (pool-6-thread-1) --------------------------------------------------
   16:21:33,619 INFO  [org.rhq.enterprise.server.core.StartupBean] (pool-6-thread-1) JBoss Operations Network 3.3.0.GA (build 4f16df3:e347f77) Server started.
   16:21:33,619 INFO  [org.rhq.enterprise.server.core.StartupBean] (pool-6-thread-1) --------------------------------------------------
   ```
   
4. Jalankan __RHQ Agent__ dalam mode console dan DEBUG juga. Untuk menjalankan RHQ Agent dalam mode console kita __tidak__ bisa
   melakukannya dengan perintah `rhqctl console --agent` tapi kita lakukan seperti ini:
   Masuk dulu ke direktori RHQ Agent yaitu di `/home/jboss/rhq-agent`
   
   ```
   cd /home/jboss/rhq-agent/bin
   export RHQ_CONTROL_DEBUG=true
   ./rhq-agent.sh start
   ```
   
   Ouput:
   ```
   RHQ 4.12.0.JON330GA [e347f77] (Tue Nov 18 02:23:34 WIT 2014)
   The agent is waiting for plugins to be downloaded...
   sending>
   ```
   
  Cek log di RHQ Server:
   ```
   06:17:55,795 INFO  [org.rhq.enterprise.server.core.CoreServerServiceImpl] (http-/0.0.0.0:7080-1) Agent [AGENT-X][4.12.0.JON330GA(e347f77)] would like to connect to this server
   06:17:56,058 INFO  [org.rhq.enterprise.server.core.CoreServerServiceImpl] (http-/0.0.0.0:7080-1) Agent [AGENT-X] has connected to this server at Mon Dec 15 06:17:56 WIT 2014
   06:17:57,367 INFO  [org.rhq.enterprise.server.core.CoreServerServiceImpl] (http-/0.0.0.0:7080-1) Got agent registration request for existing agent: AGENT-X[192.168.1.101:16163][4.12.0.JON330GA(e347f77)] - Will not regenerate a new token
   ```
   
   Ketikan `help` pada prompt RHQ Agent tersebut. 
   
   ```
       help
        avail: Get availability of inventoried resources
       config: Manages the agent configuration
        debug: Provides features to help debug the agent.
    discovery: Asks a plugin to run a server scan discovery
     download: Downloads a file from the RHQ Server
    dumpspool: Shows the entries found in the command spool file
         exit: Shuts down the agent's communications services and kills the agent
     failover: Provides HA failover functionality
           gc: Helps free up memory by invoking the garbage collector
    getconfig: Displays one, several or all agent configuration preferences
         help: Shows help for a given command
     identify: Asks to identify a remote server
    inventory: Provides information about the current inventory of resources
     listdata: Lists and optionally deletes agent data files. USE WITH CAUTION!
          log: Configures some settings for the log messages
      metrics: Shows the agent metrics
       native: Obtains native system information
           pc: Starts and stops the plugin container and all deployed plugins
         ping: Pings the RHQ Server
         piql: Executes a PIQL query to search for running processes
      plugins: Updates the agent plugins with the latest versions from the server
         quit: Shuts down the agent's communications services and kills the agent
     register: Registers this agent with the RHQ Server
    schedules: Retrieves measurement schedule information for the specified resource
       sender: Controls the command sender to start or stop sending commands
    setconfig: Sets an agent configuration preference
        setup: Sets up the agent configuration by asking a series of questions
     shutdown: Shuts down all communications services without killing the agent
        sleep: Puts the agent prompt to sleep for a given amount of seconds.
        start: Starts the agent comm services so it can accept remote requests
        timer: Times how long it takes to execute another prompt command
       update: Provides agent update functionality
      version: Shows information on agent version and agent environment
  ```
  
   Cobalah untuk menjalankan beberapa perintah berikut:
   
       1. version :  melihat versi dari RHQ Agent dan JDK
       2. getconfig :  melihat konfigurasi yang digunakan oleh RHQ Agent 
       3. avail :  melihat laporan mengenai semua resource yang ada di host tempat Agent berjalan
       4. inventory --types :  melihat semua jenis tipe resource (bukan tipe yang ada di inventory)
       5. inventory :  melihat semua resource yang ada di inventory
       6. discovery : memerintahkan plugin untuk melakukan scanning terhadap resource yang ada, tanpa melaporkan ke serber 
       7. discovery --full :  hampir sama dengan perintah discovery tapi akan melaporkannya ke server
       8. ping :  tes koneksi ke server
       9. metrics : melihat metrics dari agent
       10. plugins info :  melihat daftar semua plugins yang ada di agent
       11. plugins update :  memerintahkan agent untuk melakukan update plugins
     
5. Sekarang kita ceck proses yang terkait dengan JON dengan perintah `ps -Ax`
   Begini kira-kira outputnya:
   
   ```
   2003 ttys000    1:08.65 /xxx/1.6.0.jdk/bin/java -ea -javaagent:./../lib/jamm-0.2.5.jar <DELETED> -Djava.rmi.server.hostname=localhost -Dcom.sun.management.jmxremote.port=7299 -Dcom.sun.management.jmxremote.ssl=false -Dcom.sun.management.jmxremote.authenticate=false -Djava.net.preferIPv4Stack=true -Dlog4j.configuration=log4j-server.properties -Dlog4j.defaultInitOverride=true -Dcassandra-pidfile=/home/jboss/jon-server-3.3.0.GA/rhq-storage/bin/cassandra.pid -cp <DELETED> org.apache.cassandra.service.CassandraDaemon
   2061 ttys000    0:00.02 /bin/sh /x/jon-server-3.3.0.GA/jbossas/bin/standalone.sh -P /x/jon-server-3.3.0.GA/bin/rhq-server.properties
   2158 ttys000    2:31.13 /xxx/1.6.0.jdk/bin/java -D[Standalone] -XX:+UseCompressedOops -verbose:gc <DELETED> -Drhq.server.home=/x/jon-server-3.3.0.GA -Djboss.server.log.dir=/x/jon-server-3.3.0.GA/logs <DELETED> -jar /x/jon-server-3.3.0.GA/jbossas/jboss-modules.jar -mp /x/jon-server-3.3.0.GA/modules:/x/jon-server-3.3.0.GA/jbossas/modules -jaxpmodule javax.xml.jaxp-provider org.jboss.as.standalone -Djboss.home.dir=/x/jon-server-3.3.0.GA/jbossas -Djboss.server.base.dir=/x/jon-server-3.3.0.GA/jbossas/standalone -P /x/jon-server-3.3.0.GA/bin/rhq-server.properties
   2251 ttys000    0:29.16 /xxx/1.6.0.jdk//bin/java -Djava.endorsed.dirs=/x/rhq-agent/lib/endorsed -Djava.library.path=/x/rhq-agent/lib -Xms64m -Xmx128m -Djava.net.preferIPv4Stack=true -Drhq.preferences.file=/x/rhq-agent/conf/agent-prefs.properties -Dlog4j.configuration=log4j.xml -cp <DELETED> org.rhq.enterprise.agent.AgentMain --daemon
   ```
   Kita bisa lihat ada 3 proses yang dijalankan JON yaitu Storage Node (Casandra), JON Server, JON Agent.
   Proses dengan pid 2158 adalah child proses dari JON Server yang memiliki pid 2061. Kita bisa cek dengan perintah `pgrep -P 2061`
   
6. Sekarang kita coba restart JON, artinya kita akan stop dan start semua (keempat) proses diatas.
   Untuk start gunakan perintah `rhqctl stop`
   Untuk start gunakan perintah `rhqctl start`
   Selain cara itu ada juga perintah `rhqctl restart`

   Masing-masing komponen tersebut diatas bisa kita start, stop atau restart secara terpisah yaitu dengan perintah

   rhqctl stop --server
   rhqctl start --server


   Untuk stop Agent, kita perlu sedikit kerja kotor (manual), yaitu `kill -9 <proses id>` si Agent. Sedangkan untuk start, 
   kita bisa masuk ke direktori `rhq-agent/bin` dan menjalankan perintah `rhq-agent.sh -l`
   
7. Sekarang kita lihat juga port-port apa saja yang digunakan oleh JON. 
   Kita bisa gunakan perintah lsof di Linux/*nix/OS-X.
   Sebelumnya kita cek dulu proses ID dari JON
  
   ```
    23:49:31,084 INFO  [org.jboss.modules] JBoss Modules version 1.3.3.Final-redhat-1
    RHQ Storage Node               (pid 3585   ) is ?running
    RHQ Server                     (pid 3643   ) is ✔running
    JBossAS Java VM child process  (pid 3740   ) is ✔running
    RHQ Agent                      (pid 3832   ) is ✔running
   ```
   Sekarang kita cek list port yang LISTEN

   ```
   lsof -iTCP  -P | egrep LISTEN
   ```
   
   Ini contoh output di laptop saya:
   
   ```
    GitHub     578 ejlp12    5u  IPv4 0xce811577925c812b      0t0  TCP localhost:25035 (LISTEN)
    GitHub     578 ejlp12    6u  IPv6 0xce8115777dbc18a3      0t0  TCP localhost:25035 (LISTEN)
    soffice   1105 ejlp12   18u  IPv4 0xce8115779163e943      0t0  TCP *:1599 (LISTEN)
    java      3585 ejlp12   59u  IPv4 0xce8115777da1112b      0t0  TCP *:52250 (LISTEN)
    java      3585 ejlp12   61u  IPv4 0xce8115777f77a943      0t0  TCP *:7299 (LISTEN)
    java      3585 ejlp12   62u  IPv4 0xce8115777e27012b      0t0  TCP *:52251 (LISTEN)
    java      3585 ejlp12   69u  IPv4 0xce8115777e271943      0t0  TCP 192.168.1.101:7100 (LISTEN)
    java      3585 ejlp12   98u  IPv4 0xce8115777e280943      0t0  TCP 192.168.1.101:9142 (LISTEN)
    java      3740 ejlp12  288u  IPv4 0xce8115777d8ce943      0t0  TCP *:7080 (LISTEN)
    java      3740 ejlp12  325u  IPv4 0xce8115777d620943      0t0  TCP localhost:6999 (LISTEN)
    java      3740 ejlp12  330u  IPv4 0xce8115777d9d5943      0t0  TCP localhost:3447 (LISTEN)
    java      3740 ejlp12  334u  IPv4 0xce8115777d62212b      0t0  TCP localhost:6990 (LISTEN)
    java      3740 ejlp12  338u  IPv4 0xce811577924fe12b      0t0  TCP localhost:2528 (LISTEN)
    java      3740 ejlp12  339u  IPv4 0xce8115777db5f943      0t0  TCP *:7443 (LISTEN)
    java      3740 ejlp12  391u  IPv4 0xce8115777db6d12b      0t0  TCP localhost:4449 (LISTEN)
    java      3740 ejlp12  396u  IPv4 0xce8115777e6b812b      0t0  TCP localhost:4455 (LISTEN)
    java      3740 ejlp12  598u  IPv4 0xce8115777ef24943      0t0  TCP localhost:7079 (LISTEN)
    java      3832 ejlp12   59u  IPv4 0xce8115777d621943      0t0  TCP 192.168.1.101:16163 (LISTEN)
   ```
   
   Coba buat daftar port yang dibuka oleh semua komponen RHQ.
   
     - RHQ Storage Node: 52250, 7299, 52251, 7100, 9142
     - RHQ Server: 7080, 6999, 3447, 6990, 2528, 2528, 7443, 4449, 4455, 7079
     - RHQ Agent: 16163

   Port-port tersebut bisa kita ubah, konfigurasi port tersebut ada di file-file berikut di direktori `bin/`
  
     - `rhq-storage.properties`
     - `rhq-server.properties`

   Sedangkan untuk Agent, file konfigurasi ada di `hq-agent/conf/agent-configuration.xml`
   Silakan untuk mengeksplorasi file konfigurasi tersebut.

8. Selesai LAB ini!
