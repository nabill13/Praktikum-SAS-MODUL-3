# **Praktikum Modul 3**

#### SISTEM ADMINISTRASI SERVER

##### Kelompok 2

- Nabila Nur Amalia_1202190013

- Evyra Rizki Safitri_1202190015

- M. Pradata Yuda P_1202190061

  

Buatlah Subdomain dev.vm.local dengan aturan sebagai berikut :

* Ansible
* Menggunakan lxc yang sama seperti vm.local
* Folder harus berada pada var/www/html/dev{name_app}



### **Praktikum**

* Pergi ke directory menggunakan ansible

  ![2-1.PNG](https://github.com/nabill13/Praktikum-SAS-MODUL-3/blob/main/ansible/ansible/2-1.PNG?raw=true)


  ```
  ---
  - hosts: all
    become : yes
    tasks:
      - name: install bind9 dan dnsutils
        apt:
         pkg:
           - bind9
           - dnsutils
  ```



* Kemudian install packages menggunakan ansible

  ![3.PNG](https://github.com/nabill13/Praktikum-SAS-MODUL-3/blob/main/ansible/ansible/3.PNG?raw=true)

  



* Membuat file config.yml 

  ![4.PNG](https://github.com/nabill13/Praktikum-SAS-MODUL-3/blob/main/ansible/ansible/4.PNG?raw=true)
  
  
  ```
  ---
  - hosts: all
    become : yes
    tasks:
     - name: membuat direktori
       file:
        path: /var/www/html/dev/landing
        state: directory
     - name: copy file vm.local
       copy:
        src: /etc/bind/vm/vm.local
        dest: /var/www/html/dev/landing
     - name: mengganti konfigurasi
       replace:
        path: /var/www/html/dev/landing/vm.local
        regexp: 'www'
        replace: 'dev'
     - name: copy file named.conf.local
       copy:
        src: /etc/bind/named.conf.local
        dest: /etc/bind/named.conf.local
     - name: mengganti konfigurasi conf local
       replace:
        path: /etc/bind/named.conf.local
        regexp: '/etc/bind/vm/vm.local'
        replace: '/var/www/html/dev/landing/vm.local'
     - name: mengganti konfigurasi conf local part2
       replace:
        path: /etc/bind/named.conf.local
        regexp: '/etc/bind/vm/115.168.192.in-addr.arpa'
        replace: '/var/www/html/dev/landing/115.168.192.in-addr.arpa'
     - name: copy file 115.168.192.in-addr.arpa
       copy:
        src: /etc/bind/vm/115.168.192.in-addr.arpa
        dest: /var/www/html/dev/landing
     - name: copy file resolv.conf
       copy:
        src: /etc/resolv.conf
        dest: /etc/resolv.conf
     - name: copy file named.conf.options
       copy:
        src: /etc/bind/named.conf.options
        dest: /etc/bind/named.conf.options
     - name: restart nginx
       service:
        name: nginx
        state: restarted
     - name: restart bind9
       action: service name=bind9 state=restarted
  ```
  
* Gunakan script berikut untuk melakukan instalasi

  ![5.PNG](https://github.com/nabill13/Praktikum-SAS-MODUL-3/blob/main/ansible/ansible/5.PNG?raw=true)
  
* Tambahkan subdomain pada /etc/hosts

  ![6.PNG](https://github.com/nabill13/Praktikum-SAS-MODUL-3/blob/main/ansible/ansible/6.PNG?raw=true)
  
* Buka file vm.local 

  ![7.PNG](https://github.com/nabill13/Praktikum-SAS-MODUL-3/blob/main/ansible/ansible/7.PNG?raw=true)
  
* Tambahkan baris www. -> kemudian jika sudah keluar dari lxc

  ![8.PNG](https://github.com/nabill13/Praktikum-SAS-MODUL-3/blob/main/ansible/ansible/8.PNG?raw=true)
  
* Buka dan edit vm.local pada directory /etc/nginx/sites-enabled/

  ![9.PNG](https://github.com/nabill13/Praktikum-SAS-MODUL-3/blob/main/ansible/ansible/9.PNG?raw=true)
  
  

  ![10.PNG](https://github.com/nabill13/Praktikum-SAS-MODUL-3/blob/main/ansible/ansible/10.PNG?raw=true)
  
* Kemudian buka dan edit vm.local di directory /etc/bind/vm/

  ![11-01.PNG](https://github.com/nabill13/Praktikum-SAS-MODUL-3/blob/main/ansible/ansible/11-01.PNG?raw=true)
  
* Jika sudah restart semua packages

  ![11-1.PNG](https://github.com/nabill13/Praktikum-SAS-MODUL-3/blob/main/ansible/ansible/11-1.PNG?raw=true)
  
  
  

- Buka Wifi Settings, tambahkan dns server dan uncheck otomati pada menu Ipv4

  ![setting.PNG](https://github.com/nabill13/Praktikum-SAS-MODUL-3/blob/main/ansible/ansible/setting.PNG?raw=true)
  
- Pada menu IPv6, check disabled (kita harus mematikan IPv6 terlebih dahulu), kemudian click apply

- Coba reconnecting kembali ke wifi, dan cek dev.vm.local pada browser 


![hasil.png](https://github.com/nabill13/Praktikum-SAS-MODUL-3/blob/main/ansible/ansible/hasil.png?raw=true)
