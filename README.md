# Born2beroot

## 📝 Proje Açıklaması

Bu projede, bir Linux sunucusunun güvenliğini sağlamak ve üzerinde WordPress kurmak için gereken tüm adımlar gerçekleştirilecektir. Parola politikalarından disk bölümlendirmesine, firewall yapılandırmasından bonus servis kurulumlarına kadar tüm süreçler titizlikle ele alınacaktır. Proje sonunda, zorunlu gereksinimler hatasız çalışmalı, bonuslar ise ekstra puan için uygulanabilir olmalıdır.

> **"Sunucunuzun güvenliği, en zayıf halkası kadar güçlüdür."**

## 📋 İçindekiler

1. [Giriş](#giriş)
2. [Zorunlu Gereksinimler](#zorunlu-gereksinimler)
3. [Bonus Kısım](#bonus-kısım)
4. [Teslim ve Değerlendirme](#teslim-ve-değerlendirme)
5. [Yol Haritası](#yol-haritası)
6. [Referanslar & Örnekler](#referanslar--örnekler)

## 🚀 Giriş

Bu proje kapsamında, sanal bir makine üzerinde güvenlik politikaları uygulanacak, disk bölümlendirmesi ve servis kurulumları gerçekleştirilecektir. Projenin sonunda, sunucunuz hem güvenli hem de işlevsel bir hale gelmiş olacak.

## 📌 Zorunlu Gereksinimler

### Parola Politikası

- Kullanıcı adı parolada geçmemeli.
- Root parolası hariç, yeni parolalar eski paroladan en az 7 karakter farklı olmalı.
- Tüm kullanıcıların ve root'un parolası bu politikaya uygun olarak değiştirilmeli.

### Sudo Yapılandırması

- Sudo ile kimlik doğrulama 3 deneme ile sınırlandırılmalı.
- Yanlış parola girildiğinde özel bir hata mesajı gösterilmeli.
- Her sudo işlemi (girdi/çıktı) `/var/log/sudo/` altında arşivlenmeli.
- TTY modu zorunlu olmalı.
- Sudo ile kullanılabilecek PATH şu şekilde sınırlandırılmalı:
  ```
  /usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/snap/bin
  ```

### Monitoring Script

- Sunucu açılışında ve her 10 dakikada bir, tüm terminallere aşağıdaki bilgiler `wall` ile yayınlanmalı:
  - Sistem mimarisi ve kernel versiyonu
  - Fiziksel ve sanal işlemci sayısı
  - RAM ve disk kullanımı (yüzde olarak)
  - CPU kullanım yüzdesi
  - Son açılış tarihi/saat
  - LVM aktif mi
  - Aktif bağlantı sayısı
  - Aktif kullanıcı sayısı
  - IPv4 ve MAC adresi
  - Sudo ile çalıştırılan komut sayısı

  ![image1](image1)

- Script savunmada açıklanmalı ve modifikasyon olmadan durdurulabilmeli (örn. cron kullanımı).

### Firewall & Servis Ayarları

- SSH servisi 4242 portunda çalışmalı.
- Firewalld (Rocky) veya UFW (Debian) ile yalnızca gerekli portlar açık olmalı.
- AppArmor (Debian) veya SELinux (Rocky) aktif olmalı.

  ![image2](image2)
  ![image3](image3)

### Disk Bölümlendirme

- LVM ile aşağıdaki gibi bir yapılandırma önerilir:

  ![image4](image4)

## ⭐ Bonus Kısım

**Bonuslar sadece zorunlu kısım %100 tamamlanınca değerlendirilir!**

- Lighttpd, MariaDB ve PHP ile çalışan bir WordPress sitesi kurulmalı.
- NGINX veya Apache2 hariç, kendi seçtiğiniz bir ekstra servis kurulmalı ve savunmada neden seçtiğiniz açıklanmalı.
- Ekstra servisler için gerekli portlar açılmalı ve firewall kuralları güncellenmeli.

## 📦 Teslim ve Değerlendirme

- Sadece `signature.txt` dosyası teslim edilmeli. VM dosyası paylaşmak yasak!
- İmza almak için:
  - **Linux:** `sha1sum rocky_serv.vdi`
  - **Windows:** `certUtil -hashfile rocky_serv.vdi sha1`
  - **MacOS/M1:** `shasum rocky_serv.vdi` veya `shasum rocky.utm/Images/disk-0.qcow2`
- Savunmada, `signature.txt` ile VM imzası karşılaştırılacak. Farklıysa notunuz **0**.
- **Snapshot kullanmak kesinlikle yasaktır!** Tespit edilirse notunuz **0** olur.

## 🗺️ Yol Haritası

1. **Sistem Kurulumu**
    - Temiz kurulum yap, güncellemeleri uygula.
    - LVM ile disk bölümlendirmesini ayarla (örnek görseli takip et).
2. **Güvenlik Politikaları**
    - Parola politikalarını PAM ile uygula.
    - Tüm kullanıcıların ve root'un parolasını değiştir.
3. **Sudo Konfigürasyonu**
    - `/etc/sudoers` veya `sudoers.d` ile gerekli ayarları yap.
    - Özel hata mesajı ve PATH kısıtlamasını ekle.
    - TTY ve loglamayı aktif et.
4. **Monitoring Script**
    - Bash ile monitoring.sh yaz.
    - Cron ile açılışta ve 10 dakikada bir çalışmasını sağla.
    - `wall` ile bilgi paylaşımını uygula.
5. **Firewall ve Servisler**
    - SSH portunu değiştir (4242).
    - UFW veya firewalld ile sadece gerekli portları aç.
    - SELinux/AppArmor etkin olsun.
6. **Bonuslar**
    - Lighttpd + MariaDB + PHP ile WordPress kurulumunu yap.
    - Seçtiğin ekstra servisi kur ve gerekçeni hazırla.
    - Firewall kurallarını güncelle.
7. **Teslim**
    - VM disk imzasını al, `signature.txt` dosyasına ekle.
    - Son bir kontrol ile snapshot olmadığından emin ol.

## 📚 Referanslar & Örnekler

- [Görsel 1: Monitoring Script Çıktısı](#)  
  ![image1](image1)
- [Görsel 2: Rocky Linux Servisler](#)
  ![image2](image2)
- [Görsel 3: Debian Servisler](#)
  ![image3](image3)
- [Görsel 4: LVM Disk Yapısı](#)
  ![image4](image4)

---

Bu proje, 42 Okulu müfredatının bir parçasıdır.
