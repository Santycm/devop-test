# Dockerfile (frontend)
FROM node:20

WORKDIR /app

COPY package*.json ./

# Instala dependencias sin correr scripts de instalación aún
RUN npm install --ignore-scripts

# Copia el resto del código
COPY . .

# Reinstala esbuild de forma segura
RUN npm rebuild esbuild

# Compila la app (si es necesario)
RUN npm run build

EXPOSE 5173

CMD ["npm", "run", "dev"]
