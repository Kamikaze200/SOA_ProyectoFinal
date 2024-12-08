# Etapa 1: Construcción
FROM gradle:7.6-jdk11 AS build

# Establece el directorio de trabajo
WORKDIR /app

# Copia los archivos de Gradle (para aprovechar el cacheo de dependencias)
COPY gradle /app/gradle
COPY gradlew /app
COPY build.gradle /app
COPY settings.gradle /app
COPY gradle.properties /app  # Copiar este archivo si existe

# Otorga permisos de ejecución al archivo gradlew
RUN chmod +x gradlew

# Copia el código fuente del proyecto antes de la primera compilación
COPY src /app/src

# Descarga las dependencias de Gradle (sin compilar aún)
RUN ./gradlew build --no-daemon --warning-mode all

# Construcción del proyecto (ahora que todo el código fuente está copiado)
RUN ./gradlew build --no-daemon --warning-mode all


# Etapa 2: Imagen final
FROM openjdk:11-jre-slim

# Establece el directorio de trabajo
WORKDIR /app

# Copia el archivo JAR construido en la etapa anterior
COPY --from=build /app/build/libs/*.jar app.jar

# Exponer el puerto en el que Spring Boot ejecutará la aplicación (puerto por defecto 8080)
EXPOSE 8093

# Comando para ejecutar la aplicación
ENTRYPOINT ["java", "-jar", "/app/app.jar"]