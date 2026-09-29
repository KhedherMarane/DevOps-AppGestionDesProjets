# Étape 1 : build Angular
FROM node:22-alpine AS build
WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY . .
RUN npm run build

# Étape 2 : serveur nginx
FROM nginx:alpine
COPY --from=build /app/dist/*/browser /usr/share/nginx/html
EXPOSE 80
