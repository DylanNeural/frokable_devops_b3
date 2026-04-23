# ─────────────────────────────────────────────────────────────────────────────
# Dockerfile — Frontend React/Vite
#
# CE FICHIER EST VOLONTAIREMENT VIDE.
# Votre mission : écrire le Dockerfile qui conteneurise le front.
#
# Critères attendus :
#   - Utiliser un multi-stage build (obligatoire ici) :
#       Stage 1 "builder" : image node:20-alpine, installer les dépendances,
#                           builder l'app (npm run build → dossier dist/)
#       Stage 2 "runner"  : image nginx:alpine, copier le dist/ dans
#                           /usr/share/nginx/html, exposer le port 8080
#
# Pourquoi multi-stage ? L'image finale ne doit pas contenir Node ni
# node_modules — juste les fichiers statiques servis par Nginx.
# ─────────────────────────────────────────────────────────────────────────────

FROM node:20-alpine AS builder
WORKDIR /app

# Variable injectee au build pour definir l'URL API consommee par le frontend.
ARG VITE_API_URL=http://localhost:8000
ENV VITE_API_URL=${VITE_API_URL}

# Copie d'abord les manifests npm pour optimiser le cache Docker.
COPY package.json package-lock.json ./
RUN npm ci
# Puis copie du code source et build Vite vers /app/dist.
COPY . .
RUN npm run build

# Image finale minimale: Nginx sert uniquement les fichiers statiques.
FROM nginx:alpine AS runner
# Configuration Nginx (port 8080 + fallback SPA).
COPY nginx.conf /etc/nginx/nginx.conf
# Recupere uniquement le resultat du build (pas Node, pas node_modules).
COPY --from=builder /app/dist /usr/share/nginx/html
# Port HTTP expose par Nginx dans le conteneur.
EXPOSE 8080
