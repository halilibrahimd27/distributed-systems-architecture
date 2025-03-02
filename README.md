# Dağıtık Sistemler

Bu proje, Docker teknolojisi kullanılarak oluşturulmuş dağıtık bir sistem mimarisidir. Projede, Nginx yük dengeleyici, iki adet replikasyonlu Spring Boot uygulama sunucusu, PostgreSQL veritabanı sunucusu ve Redis cache sunucusu yer almaktadır.

## İçerik

- [Proje Hakkında](#proje-hakkında)
- [Mimari Tasarım](#mimari-tasarım)
- [Ön Koşullar](#ön-koşullar)
- [Kurulum ve Çalıştırma](#kurulum-ve-çalıştırma)
- [Proje Yapısı](#proje-yapısı)
- [Konfigürasyon Detayları](#konfigürasyon-detayları)
- [Sorun Giderme](#sorun-giderme)

## Proje Hakkında

Bu proje, modern uygulama geliştirme ve dağıtık sistem mimarilerini öğrenmek amacıyla hazırlanmıştır. Docker Compose kullanılarak, tüm servisler (Nginx, Spring Boot uygulamaları, PostgreSQL ve Redis) container'lar içerisinde konfigüre edilmiştir. Ayrıca, Nginx yapılandırması ile failover mekanizması sağlanarak, herhangi bir uygulama sunucusu devre dışı kaldığında isteklerin diğer sunucuya yönlendirilmesi garanti altına alınmıştır.

## Mimari Tasarım

- **Nginx:**  
  - Yük dengeleyici olarak çalışır ve gelen istekleri Spring Boot uygulama sunucularına yönlendirir.
  - Failover yapılandırması sayesinde, bir uygulama sunucusunun erişilememesi durumunda diğer sunucu devreye girer.

- **Spring Boot Uygulamaları :**  
  - İki adet replikasyonlu uygulama sunucusu bulunur.
  - Uygulama sunucuları, PostgreSQL veritabanı ve Redis cache ile entegre çalışmaktadır.

- **PostgreSQL:**  
  - Uygulama verilerinin saklanması için kullanılır.
  - `pg_data` adlı Docker volume ile verilerin kalıcılığı sağlanır.

- **Redis:**  
  - Uygulama performansını artırmak için cache mekanizması sağlar.

## Ön Koşullar

- [Docker Engine](https://docs.docker.com/get-docker/) kurulmuş olmalı.
- [Docker Compose](https://docs.docker.com/compose/install/) yüklü olmalı.
- (Opsiyonel) Git kurulumu, repository’yi klonlamak için önerilir.

## Kurulum ve Çalıştırma

1. **Repository’yi Klonlayın:**

   ```bash
   git clone <repository_url>
   cd <repository_directory>
   ```

2. **Docker Container’larını Oluşturun ve Başlatın:**

   ```bash
   docker-compose up --build -d
   ```

   Bu komut, Spring Boot uygulamaları için gerekli imajları oluşturur ve tüm container’ları başlatır.

3. **Uygulamaya Erişim:**

   Tarayıcınızda `http://localhost` adresine giderek, Nginx yük dengeleyici aracılığıyla uygulamaya erişebilirsiniz.

4. **Container’ları Durdurmak:**

   ```bash
   docker-compose down
   ```

   komutuyla tüm container’ları kapatabilirsiniz.

## Proje Yapısı

```
.
distributed-systems-architecture/
│-- app/
│   ├── src/
│   │   ├── main/
│   │   │   ├── java/com/example/app2/
│   │   │   │   ├── controller/
│   │   │   │   │   ├── UserController.java
│   │   │   │   ├── model/
│   │   │   │   │   ├── User.java
│   │   │   │   ├── repository/
│   │   │   │   │   ├── UserRepository.java
│   │   │   │   ├── service/
│   │   │   │   │   ├── UserService.java
│   │   │   │   ├── Application.java
│   │   ├── resources/
│   │   │   ├── application.properties
│   ├── target/
│   │   ├── classes/
│   │   ├── generated-sources/
│   │   ├── generated-test-sources/
│   │   ├── maven-archiver/
│   │   ├── maven-status/
│   │   ├── test-classes/
│   │   ├── app-1.0.0.jar
│   │   ├── app-1.0.0.jar.original
│   ├── pom.xml
│-- db/
│   ├── init.sql
│-- nginx/
│   ├── nginx.conf
│-- docker-compose.yml
│-- Dockerfile

│-- README.md
│-- video/
│   ├── setup-demo.mp4
```

## Konfigürasyon Detayları

- **Nginx:**  
  - Konfigürasyon dosyası: nginx/nginx.conf 
  - Reverse Proxy olarak çalışır: Tüm HTTP isteklerini Spring Boot uygulamasına yönlendirir.
  - Load Balancer olarak ayarlanmıştır: Eğer birden fazla backend eklenirse, istekleri dağıtabilir.
  - Failover mekanizması içerir: Eğer app container'ı kapanırsa, uygun yanıt döndürür.

- **Spring Boot Uygulamaları :**  
  - Port: 8080
  - Veritabanı: PostgreSQL (db container'ı)
  - Cache: Redis (redis container'ı)
  - Docker Container içinde çalıştırılır.

- **PostgreSQL:**  
  - Konteyner Adı: db
  - Port: 5432
  - Veritabanı Adı: mydb
  - Kullanıcı Adı: user
  - Şifre: password
  - Veri kaybını önlemek için pg_data adlı Docker volume kullanılır.

- **Redis:**  
  - Konteyner Adı: redis
  - Port: 6379
  - Spring Boot tarafından cache olarak kullanılır.
    
## Sorun Giderme

- **Docker ve Docker Compose Kurulumu:**
  - Docker ve Docker Compose’un güncel ve doğru şekilde kurulu olduğundan emin olun.

- **Container Logları:**
  - Herhangi bir servis başlatılamıyorsa, ilgili container’ın loglarını inceleyin:

    ```bash
    docker logs <container_name>
    ```

- **Port Çakışmaları:**
  - Özellikle `80`, `5432` ve `6379` portlarının başka servisler tarafından kullanılmadığından emin olun.

