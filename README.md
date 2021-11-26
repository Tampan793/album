# RatuFileSaver2
Bot menghasilkan tautan yang dapat dibagikan di dalam telegram untuk video, photo, dokumen dan bisa berbagi secara grup.
<hr>

Instalasi yang dibutuhkan
1. NGINX</br>
2. SSL</br>
3. Domain</br>
4. NodeJS</br>
5. PM2 <code>npm install -g pm2</code></br>
6. NPM yang dibutuhkan. Jangan lupa pasang di tempat package.json disimpan di folder bot</br>
<code>npm install filsaverobot</code></br>
<code>npm install dotenv</code></br>
<code>npm install mongodb</code></br>
<code>npm install nodemon</code></br>
<code>npm install telegraf</code></br>
<hr>

<b>~ WINDOWS ~</b></br>
<b>LANGKAH 1</b></br>
- NGINX</br>

Sebelum anda pasang bot download dulu nginx versi windows <a href="https://nginx.org/en/download.html">di sini</a>.</br>
Setelah download selesai extraxt zip dan pindahkan ke folder C:\ dan jangan lupa rename nama folder nginxnya menjadi "nginx". Jalankan nginxnya di cmd dengan perintah berikut.

    C:\nginx\.\nginx.exe -s start

<b>LANGKAH 2</b></br>
- SSL</br>

Buat terlebih dahulu SSL dan pastikan Anda punya ip publik dan domain yang terhubung ke server dan bisa diakses darimana saja.</br>

Karena servernya Windows kami menyarankan Anda membuat ssl di web ini <a href="https://punchsalad.com/ssl-certificate-generator/">Free SSL Certificate Generator</a> dan tutorial ada disini <a href="https://punchsalad.com/ssl-certificate/install-lets-encrypt-godaddy/#chapter2">Tutorial</a>. Lewatkan pada bagian tutorial cpanel, download file yang dibutuhkan disana dan taruh di folder C:\nginx\html lalu akses tautan vertifikasi untuk mendaptkan .crt dan .key. dan simpan ke folder C:\nginx\ssl.</br>

Buka file di folder nginx-conf yang sudah Anda unduh lalu buka lagi folder windows dan buka file bot.conf menggunakan editor di windows. Anda akan melihat kode dibawah, ganti tulisan <b>MY_DOMAIN</b> dengan domain Anda, karena kami sudah menempatkan PATH folder ssl maka tinggal Anda taruh saja sesuai dengan PATH yang kami tulis.


    server {
        listen 80;
        server_name MY_DOMAIN;
        return 301 https://$server_name$request_uri;
    }

    server {
        listen 443 ssl;
        server_name MY_DOMAIN;

        error_log C:/nginx/bot.error.log error;

        ssl_session_timeout 5m;

        ssl_certificate C:/nginx/html/ssl/ca-bundle.crt;
        ssl_certificate_key C:/nginx/html/ssl/private-key.key;

        location / {
            proxy_set_header Host $http_host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header X-Forwarded-Proto $scheme;

            proxy_pass http://localhost:8443;
        }
    }


Jika sudah di ganti pindahkan semua isi yang ada di folder windows tadi ke folder C:\nginx\conf lalu di cmd ketik ini.
    
    C:\nginx\.\nginx.exe -s reload

<b>LANGKAH 3</b></br>
- config.js</br>

Pemasangan ada di file config.js</br>

<code>DOMAIN</code> - Domain didapat dari saat anda setting nginx tadi.</br>
<code>PORT</code> - Port didapat dari saat anda setting nginx tadi.</br>
<code>TOKEN</code> - Dapatkan Token Bot dari Bot father.</br>
<code>ADMIN</code> - ID Akun Anda (jika Anda tidak dapat menemukannya menggunakan bot seperti @getmyid_bot). <b>Jika ada tambahan ADMIN1 dan ADMIN2 tulis sesuai contoh yang ada di code dan tinggal tambah angka di belakangnya</b></br>

    ADMIN: '',
    ADMIN1: '',
    ADMIN2: '',

<code>BOTUSERNAME</code> - Nama pengguna bot Anda tanpa '@' dan harus huruf kecil.</br>
<code>DB_URL</code> - Buat akun di https://www.mongodb.com/cloud/atlas , nama database - RatuMediaFile ,nama collection - RatuFileBackup.Klik Connect dan pilih 'Hubungkan aplikasi Anda'.copy tautan dan ganti "< password >" dengan kata sandi pengguna yang memiliki akses ke DB dan ganti "myFirstDatabase" untuk "RatuMediaFile". Kalau mau ubah sesuai keinginan nama databasenya ada di folder config.</br>
<b>Tautan DB_URL</b>

    mongodb+srv://login:password@bot.qnbbq.mongodb.net/database?retryWrites=true&w=majority

<code>LOG_CHANNEL</code> - buat saluran pribadi dan dapatkan ID saluran (jika Anda tidak dapat meneruskan ID saluran apa pun dari saluran ke @getidsbot itu mungkin terlihat seperti -1001234567899).</br>

<b>LANGKAH 4</b></br>
Pindahkan beberapa yang ada di folder bot</br>
Pastikan ini pindah semua ke PATH C:\nginx\html\hooks\bot</br>
- database</br>
- config</br>
- package-lock.json</br>
- package.json</br>
- help.js</br>
- config.js</br>
- index.js</br>
<hr>

Jangan lupa gunakan perintah ini untuk jalankan bot. Pastikan sudah ada di folder bot, kalau belum ketik

    cd C:\nginx\html\hooks\bot pm2 start index.js --name ratufilesaver --watch --ignore-watch="node_modules"

<b>INFO</b></br>
Kalian bisa menggunakan bot tanpa domain dan SSL dengan merubah kode dibawah ini di index.js menjadi <b>bot.launch()</b>. Lewatkan saja langkah NGINX dan SSL, lewatkan saja tulisan <b>DOMAIN</b> dan <b>PORT</b> di config.js.

    bot.launch({
        webhook:{
        domain:`${config.domain}`,
        port:`${config.PORT}`
        }
    })
<hr>

<b>~ LINUX ~</b></br>
<b>LANGKAH 1</b></br>
- NGINX</br>

Sebelum anda pasang bot download dulu nginx versi linux dengan perintah.

    sudo apt install nginx -y

Lalu jalankan dengan perintah.

    sudo systemctl enable nginx && sudo systemctl start nginx

<b>LANGKAH 2</b></br>
- SSL</br>

Buat terlebih dahulu SSL dan pastikan Anda punya ip publik dan domain yang terhubung ke server dan bisa diakses darimana saja.</br>

Karena servernya Linux dan tidak ingin kesulitan kami menyarankan Anda membuat ssl di web ini <a href="https://punchsalad.com/ssl-certificate-generator/">Free SSL Certificate Generator</a> dan tutorial ada disini <a href="https://punchsalad.com/ssl-certificate/install-lets-encrypt-godaddy/#chapter2">Tutorial</a>. Lewatkan pada bagian tutorial cpanel, download file yang dibutuhkan disana dan taruh di folder /var/www/html lalu akses tautan vertifikasi untuk mendaptkan .crt dan .key. dan simpan ke folder /etc/ssl.</br>


Buka file di folder nginx-conf yang sudah Anda unduh lalu buka lagi folder linux dan buka file bot.conf menggunakan editor di komputer Anda. Anda akan melihat kode dibawah, ganti tulisan <b>MY_DOMAIN</b> dengan domain Anda, karena kami sudah menempatkan PATH folder ssl maka tinggal Anda taruh saja sesuai dengan PATH yang kami tulis.


    server {
        listen 80;
        server_name MY_DOMAIN;
        return 301 https://$server_name$request_uri;
    }

    server {
        listen 443 ssl;
        server_name MY_DOMAIN;

        error_log /etc/nginx/bot.error.log error;

        ssl_session_timeout 5m;

        ssl_certificate /etc/ssl/ca-bundle.crt;
        ssl_certificate_key /etc/ssl/private-key.key;

        location / {
            proxy_set_header Host $http_host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header X-Forwarded-Proto $scheme;

            proxy_pass http://localhost:8443;
        }
    }


Lalu jalankan dengan perintah ini dan jangan lupa bot.conf tadi sudah pindah ke folder /etc/nginx/
    
    systemctl restart nginx

<b>LANGKAH 3</b></br>
- config.js</br>

Pemasangan ada di file config.js</br>

<code>DOMAIN</code> - Domain didapat dari saat anda setting nginx tadi.</br>
<code>PORT</code> - Port didapat dari saat anda setting nginx tadi.</br>
<code>TOKEN</code> - Dapatkan Token Bot dari Bot father.</br>
<code>ADMIN</code> - ID Akun Anda (jika Anda tidak dapat menemukannya menggunakan bot seperti @getmyid_bot). <b>Jika ada tambahan ADMIN1 dan ADMIN2 tulis sesuai contoh yang ada di code dan tinggal tambah angka di belakangnya</b></br>

    ADMIN: '',
    ADMIN1: '',
    ADMIN2: '',

<code>BOTUSERNAME</code> - Nama pengguna bot Anda tanpa '@' dan harus huruf kecil.</br>
<code>DB_URL</code> - Buat akun di https://www.mongodb.com/cloud/atlas , nama database - RatuMediaFile ,nama collection - RatuFileBackup.Klik Connect dan pilih 'Hubungkan aplikasi Anda'.copy tautan dan ganti "< password >" dengan kata sandi pengguna yang memiliki akses ke DB dan ganti "myFirstDatabase" untuk "RatuMediaFile". Kalau mau ubah sesuai keinginan nama databasenya ada di folder config.</br>
<b>Tautan DB_URL</b>

    mongodb+srv://login:password@bot.qnbbq.mongodb.net/database?retryWrites=true&w=majority
    
<code>LOG_CHANNEL</code> - buat saluran pribadi dan dapatkan ID saluran (jika Anda tidak dapat meneruskan ID saluran apa pun dari saluran ke @getidsbot itu mungkin terlihat seperti -1001234567899).</br>

<b>LANGKAH 4</b></br>
Pindahkan beberapa yang ada di folder bot</br>
Pastikan ini pindah semua ke PATH /var/www/html/hooks/bot</br>
- database</br>
- config</br>
- package-lock.json</br>
- package.json</br>
- help.js</br>
- config.js</br>
- index.js</br>
<hr>

Jangan lupa gunakan perintah ini untuk jalankan bot. Pastikan sudah ada di folder bot, kalau belum ketik

    cd /var/www/html/hooks/bot pm2 start index.js --name ratufilesaver --watch --ignore-watch="node_modules"

<b>INFO</b></br>
Kalian bisa menggunakan bot tanpa domain dan SSL dengan merubah kode dibawah ini di index.js menjadi <b>bot.launch()</b>. Lewatkan saja langkah NGINX dan SSL, lewatkan saja tulisan <b>DOMAIN</b> dan <b>PORT</b> di config.js.

    bot.launch({
        webhook:{
        domain:`${config.domain}`,
        port:`${config.PORT}`
        }
    })
<hr>

<h2>Berikut adalah beberapa perintah dan penggunaan admin.</h2>

~ Bagaimana pengguna melarang, unban dan kick dari bot dan grup.
<code>/ban</code> userID caption jika ada.</br>
<code>/banchat</code> userID (pribadi).</br>
<code>/unban</code> userID.</br>
<code>/unbanchat</code> userID (pribadi).</br>
<code>/kick</code> userID.</br>
<b>Dapatkan UserID dari saluran log.</b></br>

~ Bagaimana cara menggunakan pin dan unpin di grup.</br>
<code>/pin</code> reply ke pesan yang mau di pin.</br>
<code>/unpin</code> reply ke pesan yang mau di unpin.</br>

~ Bagaimana cara kirim pesan ke pengguna dari grup.</br>
<code>/send</code> pesan. kirim pesan di grup.</br>

~ Bagaimana cara kirim pesan ke pengguna dari bot.</br>
<code>/sendchat</code> userID pesan. kirim ke pengguna melalui bot.</br>

<h2>Cara menghapus file dari bot.</h2>
Anda dapat menghapus file 4 cara.</br>

  ⚫ Hapus file individual dengan file_id.
  
  ⚫ Hapus file grup dengan mediaId.
  
  ⚫ Hapus semua file Kirim oleh pengguna.
  
  ⚫ Hapus semua file Kirim ke bot.


~ Hapus file individual dengan file_id.</br>
<code>/rem</code> file_id. Ini akan menghapus file satu per satu saat Anda memberikan file_id, dapatkan file_id dari saluran log.</br>

~ Hapus file grup dengan mediaId.</br>
<code>/remgrp</code> mediaId. Ini akan menghapus media dalam grup, dapatkan mediaId dari saluran log.</br>

~ Hapus semua file Kirim oleh pengguna.</br>
<code>/remall</code> userID. Anda dapat menghapus semua file dikirim oleh pengguna tertentu jika pengguna mengirim konten atau spam yang kasar, dapatkan userid dari saluran log.</br>

~ Hapus semua file Kirim ke B0T.</br>
<code>/clear</code>. Ini akan menghapus semua file yang dikirim ke bot secara permanen.</br>

<h2>Kirim pesan ke pengguna</h2>

<code>/broadcast</code>. Anda dapat menyiarkan pesan teks ke pengguna Anda, pesan akan dikirim dari pengguna terakhir bergabung untuk pertama-tama bergabung dengan pengguna untuk mengurangi spam. Cobalah untuk tidak mengirim terlalu banyak pesan sekaligus jika Anda memiliki sejumlah besar pengguna.

<h2>Cara mengetahui total pengguna bot.</h2>

<code>/stats</code>. Anda akan mendapatkan total pengguna memulai bot Anda, data waktu nyata akan diperbarui setelah siaran yang berhasil.
<hr>

<b>Jika Anda ingin mendukung saya, ikuti saya di GitHub sebagai dukungan.</b>

//Update

HISTORY 9
1. Perbaikan kirim media tanpa limit.

HISTORY 8
1. Perbaikan penulisan URL gabung grup/channel tinggal tulis name-https://t.me/test

HISTORY 7
1. Perbaikan dalam list grup untuk melakukan perintah didalam grup.
2. Kirim media secara grup.
3. Hapus media secara grup.

HISTORY 6
1. Kirim pesan ke pengguna melalui bot.

HISTORY 5
1. Kirim pesan ke pengguna melalui grup.

HISTORY 4
1. bot mendukung kick, ban, unban dan ada pesan pribadi.
2. bot mendukung pin dan unpin.

HISTORY 3
1. Perbaikan penulisan file_name.
2. Perbaikan pencarian media.

HISTORY 2
1. Function teks disederhanakan.
2. Mendeteksi jika belum ada nama akun akan dikosongkan.
3. Admin bisa menggunakan bot tanpa masuk channel/grup.
4. Ada log channel untuk mengetahui siapa yang ngirim dan apa deskripsi filenya.

HISTORY 1
1. Ada join channel/grup terlebih dahulu saat start, pastikan id channel/grup di ganti pada index.js dan bot harus jadi admin di grup/channel.
2. Terdapat penambahan untuk menghilangkan null supaya tidak terlihat saat tampil.
3. Ada get ID untuk cek ID akun Anda.
4. Ada pesan bot belum dimasukkan ke channel/grup tujuan.
