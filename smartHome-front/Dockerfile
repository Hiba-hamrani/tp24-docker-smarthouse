# ---------- Stage 1 : build Angular avec Node ----------
# Angular 14 est compatible avec Node 14.15+ et Node 16
FROM node:16-alpine AS builder
WORKDIR /app

COPY package*.json ./
RUN npm install

COPY . .
RUN npm run build

# ---------- Stage 2 : servir les fichiers statiques avec Nginx ----------
FROM nginx:alpine
COPY nginx.conf /etc/nginx/conf.d/default.conf
# outputPath dans angular.json = dist/smart-home
COPY --from=builder /app/dist/smart-home /usr/share/nginx/html
EXPOSE 80
