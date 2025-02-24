# Base image olarak OpenJDK 17 kullan
FROM openjdk:17-jdk-slim

# Uygulama çalıştırma dizinini belirle
WORKDIR /app

# JAR dosyasını konteynıra kopyala (Üretilen dosya adı app1-1.0.0.jar ise)
COPY target/app2-1.0.0.jar app2.jar

# Portu aç
EXPOSE 8081

# Uygulamayı çalıştır
CMD ["java", "-jar", "app2.jar"]
