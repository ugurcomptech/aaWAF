# aaWAF

Bu döküman üzerinde aaPanel'in geliştirmiş olduğu aaWAF hakkında bilgiler içermektedir. 


## WAF Nedir?


![image](https://github.com/user-attachments/assets/fc21ce82-8b50-4648-ab68-c8676a4ab8ce)


Web Application Firewall (WAF), web uygulamalarını hedef alan zararlı trafik ve siber saldırılara karşı koruma sağlayan bir güvenlik sistemidir. WAF, gelen ve giden HTTP/HTTPS isteklerini filtreleyerek SQL enjeksiyonu, XSS (Cross-Site Scripting), ve diğer yaygın web saldırılarını engeller. Bu sayede, web uygulamalarının güvenliğini artırarak veri ihlallerine ve sistem açıklarına karşı bir koruma katmanı sağlar.


## WAF Kurulumu

Aşağıda bulunan kodu sunucunuza yapıştırınız.

```
URL=https://node.aapanel.com/cloudwaf_en/scripts/install_cloudwaf_en.sh && if [ -f /usr/bin/curl ];then curl -sSO "$URL" ;else wget -O install_cloudwaf_en.sh "$URL";fi;bash install_cloudwaf_en.sh
```

![image](https://github.com/user-attachments/assets/a758a9b6-9485-4a2c-b1c4-5555310c9949)

Kurulum tamamlandıktan sonra aşağıdaki görüntülenir:

![image](https://github.com/user-attachments/assets/481da505-8127-4129-882f-f801a63f1892)


## WAF Konsol

Konsolun varsayılan bağlantı noktası 8379'dur. Sunucunun güvenlik grubu veya donanım güvenlik duvarı varsa  8379 numaralı bağlantı noktasını açmanız gerekmektedir.

Kurulum tamamlandıktan sonra, Tarayıcı erişiminde görüntülenen adresi kullanın, kullanıcı adını ve şifreyi girin, aaWAF Konsoluna Giriş Yapın

Not: Tarayıcı güvenlik soruları sorar, lütfen ona güvenin. Bunun nedeni tarayıcının kendinden imzalı sertifikaya güvenmemesidir.


![image](https://github.com/user-attachments/assets/9bdc4d08-a58d-4824-a4d9-a989ea524cd0)


Giriş sağladıktan gerekli ayarlamalar için lütfen okumaya devam ediniz.

## Web Site Ekleme


Web site >> Add Web Site sayfasına erişip domaini ve var ise SSL'inizi belirtebilirsiniz. Ekleyeceğiniz web sitenizin A kaydınız WAF'a ait olan IP adresinizi girmeniz gerekmektedir.

**Örnek:** example.com domaininizin asıl IP adresi 192.168.10.20 olarak olduğunu varsayalım. WAF sunucunuzun IP adresini de 192.168.10.25 olarak varsayalım. DNS bölgenize giriş yapıp WAF sunucunuza ait olan 192.168.10.25 adresinzi yazmanız gerekmetkedir

```
@ IN A 192.168.10.25
```

![image](https://github.com/user-attachments/assets/3e56ef3f-7bc5-46c6-9b5c-1ef9c53ac1c4)


## IP Blacklist / Whitelist

Sol tarafta bulunan menüden Black/White list sayfasına erişip gerekli işlemleri başarılı bir şekilde sağlayabilirsiniz.

![image](https://github.com/user-attachments/assets/4ff456a3-980a-4498-b7df-2cba8aabdb90)


## Custom Rules

Custom Rules sayfası üzerinden tercihlerinize göre kurallar belirleyebilirsiniz. Örneğin IP grubu engelleyebilirsiniz, IP lokasyonu, URL, URL PATH gibi bir sürü seçenek sunmakta. Tercihinize göre kullanabilirsiniz.

![image](https://github.com/user-attachments/assets/a2e28e53-4e15-45ed-b4f7-0f5bf02a505d)


## Recaptcha

Recaptcha menüsü üzerinden web sitelerinize özel veya hepsine birden Recaptcha ekleyebilirsiniz. Bu Recaptcha dilerseniz IP bazında olabilir dilerseniz URL. Tercihinize göre işlem sağlayabilirsiniz.

![image](https://github.com/user-attachments/assets/2d95273c-0c40-4ee7-9f24-b9e3d0c797c7)



## WAF Panel SSL

Erişmiş olduğunuz WAF adresine SSL sertifikası kurmak için WAF servisini durdurmamız gerekiyor.


btw default yazarak WAF'ın CLI giriş sağlıyoruz.

```
================= aaWAF CLI ============================
1) Restart WAF               2) restart Console (not affect website)

3) WAF status                4) Stop WAF

5) Start WAF                 6) View login info

```

4'e basarak WAF'ı durduruyoruz.


Aşağıdaki komutu yazarak SSL sertifikasını alıyoruz.

```
sudo certbot certonly --standalone -d waf.mydomain.com
```


Sertifika dosyalarını ekrana yazdırıyoruz.

```
cat /etc/letsencrypt/live/waf.mydomain.com/privkey.pem 
cat /etc/letsencrypt/live/waf.mydomain.com/fullchain.pem 
```



![image](https://github.com/user-attachments/assets/b33f9216-8362-4447-8570-06317b311a6a)

Panelimize IP adresimiz ile erişiyoruz ve ayarlarımızı açıyoruz. İlk önce `bind domain` kutucuğuna adresinizi yazıyoruz.


Set Certificate butonuna basıyoruz ve sertifikalarının yüklenmesi gereken sayfamız açılıyor.

![image](https://github.com/user-attachments/assets/6ba534be-e37c-4a12-abf6-fcf18508a74b)


`KEY` yazan kutucuğa `privkey.pem` dosyasının içeriğini yüklüyoruz.

`Certificate (PEM format)` yazan kutucuğa `fullchain.pem` dosyasının içeriğini yüklüyoruz ve kaydediyoruz.


btw default yazarak tekrar WAF'ın CLI giriş sağlıyoruz.

```
================= aaWAF CLI ============================
1) Restart WAF               2) restart Console (not affect website)

3) WAF status                4) Stop WAF

5) Start WAF                 6) View login info

```

5'e basarak WAF'ı tekrar başlatıyoruz ve SSL sertifikamız başarılı bir şekilde kurulmuştur.



------------------------------------------
aaWAF temel bir çözüm sunmaktadır. Daha gelişmiş bir sistem kullanmak istiyor iseniz NGINX WAF veya APACHE WAF kullanabilirsiniz. 




