FROM python:3.12-slim

ENV PYTHONDONTWRITEBYTECODE=1 \
    PYTHONUNBUFFERED=1 \
    HOST=0.0.0.0 \
    CLAIMLAB_RELOAD=false

WORKDIR /app
COPY requirements.txt pyproject.toml ./
RUN pip install --no-cache-dir --no-compile .
COPY backend ./backend
COPY alembic ./alembic
COPY alembic.ini ./alembic.ini
COPY scripts ./scripts
COPY data/processed ./data/processed
RUN chmod +x ./backend/docker-entrypoint.sh
USER 10001:10001
EXPOSE 8000
HEALTHCHECK --interval=30s --timeout=10s --start-period=20s --retries=3 \
  CMD python -c "import urllib.request; urllib.request.urlopen('http://127.0.0.1:8000/api/v1/health', timeout=5)"
ENTRYPOINT ["./backend/docker-entrypoint.sh"]
