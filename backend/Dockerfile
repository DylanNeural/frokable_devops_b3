# ─────────────────────────────────────────────────────────────────────────────
# Dockerfile — Backend Python/FastAPI
#
# CE FICHIER EST VOLONTAIREMENT VIDE.
# Votre mission : écrire le Dockerfile qui permet de conteneuriser cette API.
#
# Critères attendus :
#   - Image de base Python légère (python:3.12-slim)
#   - WORKDIR défini
#   - Dépendances installées depuis requirements.txt
#   - Code copié
#   - Port 8000 exposé
#   - Commande de démarrage : uvicorn main:app --host 0.0.0.0 --port 8000
#
# Bonus : implémenter un multi-stage build pour réduire la taille de l'image
# ─────────────────────────────────────────────────────────────────────────────


FROM python:3.12-slim AS builder
WORKDIR /app

RUN python -m venv /opt/venv
ENV PATH="/opt/venv/bin:$PATH"

COPY requirements.txt ./
RUN pip install --no-cache-dir -r requirements.txt

FROM python:3.12-slim AS runner
WORKDIR /app

ENV PATH="/opt/venv/bin:$PATH"
COPY --from=builder /opt/venv /opt/venv
COPY . .

EXPOSE 8000
CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "8000"]
