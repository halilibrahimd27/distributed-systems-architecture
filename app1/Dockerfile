# Base image olarak OpenJDK 17 kullan
FROM openjdk:17-jdk-slim

# Uygulama çalıştırma dizinini belirle
WORKDIR /app

# JAR dosyasını konteynıra kopyala
COPY target/app1-1.0.0.jar app1.jar

# Portu aç
EXPOSE 8080

# Uygulamayı çalıştır
CMD ["java", "-jar", "app1.jar"]
