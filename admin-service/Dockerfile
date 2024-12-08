# Etapa 1: Construcción
FROM gradle:7.6-jdk11 AS build

# Establece el directorio de trabajo
WORKDIR /app

# Copia los archivos de Gradle (para aprovechar el cacheo de dependencias)
COPY gradle /app/gradle
COPY gradlew /app
COPY build.gradle /app
COPY settings.gradle /app

# Otorga permisos de ejecución al archivo gradlew
RUN chmod +x gradlew

# Descarga las dependencias de Gradle (sin compilar aún)
RUN ./gradlew build --no-daemon

# Copia el código fuente del proyecto
COPY src /app/src

# Construcción del proyecto
RUN ./gradlew build --no-daemon

# Etapa 2: Imágen final
FROM openjdk:11-jre-slim

# Establece el directorio de trabajo
WORKDIR /app

# Copia el archivo JAR construido en la etapa anterior
COPY --from=build /app/build/libs/*.jar app.jar

# Exponer el puerto en el que Spring Boot ejecutará la aplicación (puerto por defecto 8080)
EXPOSE 8086

# Comando para ejecutar la aplicación
ENTRYPOINT ["java", "-jar", "/app/app.jar"]