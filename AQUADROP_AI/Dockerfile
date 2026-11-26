FROM python:3.11-slim
WORKDIR /app
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt
COPY backend ./backend
COPY frontend ./frontend
RUN mkdir -p /data/models /data/uploads
ENV AQUADROP_DB=/data/aquadrop.db AQUADROP_MODEL_DIR=/data/models AQUADROP_UPLOAD_DIR=/data/uploads PYTHONUNBUFFERED=1
WORKDIR /app/backend
# IMPORTANT: exactly ONE worker - simulation state lives in memory
CMD ["sh", "-c", "uvicorn app.main:app --host 0.0.0.0 --port ${PORT:-8000} --workers 1"]
