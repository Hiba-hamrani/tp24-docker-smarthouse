# ---------- Stage 1 : build avec Maven ----------
FROM maven:3.9-eclipse-temurin-17 AS builder
WORKDIR /app

# On copie d'abord le pom.xml pour profiter du cache Docker des dependances
COPY pom.xml .
RUN mvn -B dependency:go-offline

COPY src ./src
# -DskipTests : le test contextLoads() a besoin de MySQL, absent pendant le build
RUN mvn -B clean package -DskipTests

# ---------- Stage 2 : image finale (JRE seulement) ----------
FROM eclipse-temurin:17-jre-alpine
VOLUME /tmp
WORKDIR /
# On recupere le JAR produit dans le stage "builder" (finalName = app)
COPY --from=builder /app/target/app.jar app.jar
EXPOSE 8085
ENTRYPOINT ["java","-jar","/app.jar"]
