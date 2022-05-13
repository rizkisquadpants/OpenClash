<h1 align="center">
  <img src="https://raw.githubusercontent.com/vernesong/OpenClash/dev/img/logo.png" alt="Clash" width="200">
  <br>Config Premium OpenClash OpenWrt<br>

</h1>

  <p align="center">
    <img src="https://img.shields.io/badge/Config Version-v1.1-blue.svg">
  </a>
  <a target="_blank" href="https://github.com/vernesong/OpenClash/releases/tag/v0.45.16-beta">
    <img src="https://img.shields.io/badge/New Release-v0.45.16--beta-orange.svg">
  </a>
  </p>
  

<p align="center">
Config OpenClash OpenWrt Spesial Edition
</p>
<p align="center">
Support Multi-WAN, Pisah Traffic, Bypass Traffic, Support Game, Support Streaming
</p>
<p align="center">
- Terimakasih <a href="https://github.com/vernesong/OpenClash" target="_blank"> Vernesong </a> For Clash OpenWrt ，<a href="https://github.com/maliksih" target="_blank"> Maliksih </a> Untuk Rule Profider ，<a href="https://github.com/maliksih" target="_blank"> Reyre </a>Best Firmware  ，Dan <a href="https://github.com/maliksih" target="_blank"> Howdy </a>The Best Server -
</p>

**Konten Tabel**
  
  - [Openclash](#openclash)
    - [Edit Files Proxy Provider](#edit-files-proxy-provider)
    - [Multi-WAN ISP](#multi-wan-isp)
    - [Edit Files Rule Provider](#edit-files-rule-provider)
    - [Setting Yacd](#setting-yacd)


## Openclash

Plugin ini adalah klien Clash yang bisa dijalankan di OpenWrt. Kompatibel dengan Shadowsocks ShadowsocksR, Vmess, Trojan, Snell dan protokol lainnya, dan mengimplementasikan proxy kebijakan sesuai dengan konfigurasi aturan yang fleksibel.

 <img src="https://github.com/vernesong/OpenClash/raw/master/img/log.png">
 
### Edit Files Proxy Provider


Mengisi akun tunnel pada 3 files pada folder proxy_provider yang terdiri dari Server-SG, Server-ID, dan Server-Game.
Fungsi dari proxy_provider diatas:
* Server-SG.yaml, Untuk Akun berlokasi SINGAPORE untuk memperoleh speed/traffic yang lebih bagus.
* Server-ID.yaml, Untuk Akun berlokasi INDONESIA untuk keperluan akses websites/marketplace/live stream apps/Video on Demand yang mengharuskan memakai IP Address Publik Indonesia.
* Server-Game.yaml, Gunakan akun yang memiliki ping atau latency rendah seperti trojan,shadowsocks,grPC,Snell.



#### Keterangan lebih lanjut:

Karena config akan disetting dengan sistem fallback dimana jika proxy list pertama gagal maka akan menggunakan proxy list kedua, jika proxy list pertama dan kedua gagal maka akan menggunakan DIRECT, namun jika 30 detik kemudian proxy list pertama kembali aktif maka proxy list urutan pertama akan digunakan, sehingga sangat menguntungkan karena tidak perlu memikirkan jika salah satu proxy gagal.

### Multi-WAN ISP

Nah disini secara default menggunakan 2 ISP, dimana ISP 1 bersumber dari interface `eth1` dan ISP 2 bersumber dari `eth2`
Mengecek nama interface bisa dari LuCI > Network > Interface atau bisa jalankan commandline `ifconfig`.
Setelah memperoleh nama interface dari tiap WAN suudah di tentukan maka tinggal cara settingnya sebagai berikut:
* Edit file rizkikotet.yaml.

    Edit `interface-name: ` pada list proxy-groups dengan mengisikan nama interface WAN dari modem kalian. Dan perlu diperhatikan jika hanya menggunakan 1 WAN saja maka hapus proxy-groups `Direct ISP2` dan hapus list Pada Grup `Direct SELECT-ISP`.
    
    contoh proxy-groups dari ISP 1.
    ```yaml
    - name: Direct ISP1
        type: select
        disable-udp: false
        interface-name: eth1
        proxies:
          - DIRECT
    ```

* Edit Setiap file pada folder Proxy_Provider

    Sebagai Contoh punya 2 Akun Trojan Untuk LB 2 ISP
    ```yaml
    - name: "ID BLABLA ISP1"
      type: trojan
      server: aaa.bbb.ccc.ddd
      port: 443
      password: PASSWORD
      udp: true
      sni: BUGSNI.COM
      alpn:
        - h2
        - http/1.1
      skip-cert-verify: true
      interface-name: eth1
    
    - name: "ID BLABLA ISP2"
      type: trojan
      server: aaa.bbb.ccc.ddd
      port: 443
      password: PASSWORD
      udp: true
      sni: BUGSNI.COM
      alpn:
        - h2
         - http/1.1
      skip-cert-verify: true
      interface-name: eth2
    ```

### Edit Files Rule Provider

Untuk `rule_direct.yaml` bersifat offline Bisa Di Edit Sebagai Contoh Direct Whatsapp

Sementara untuk rule lainya bersifat online `Auto Update`

### Setting Yacd

Yacd adalah yet another clash dashboard, yakni dashboard clash yang dapat digunakan untuk mengatur proxy dan memantau services clash yang dijalankan oleh Openclash.

#### Proxies

Untuk pertama kali start openclash maka harus setting proxies `GLOBAL` ke traffic proxy-groups `RTA SERVER | Clash`.
 <img src="https://github.com/vernesong/OpenClash/raw/master/img/log.png">
