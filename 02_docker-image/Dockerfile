FROM python:3.12-alpine

ARG LAB_LOGIN
ARG LAB_TOKEN

RUN addgroup -S appgroup && adduser -S appuser -G appgroup
RUN apk add --no-cache curl

WORKDIR /app

COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt && rm requirements.txt

COPY --chown=appuser:appgroup app ./app

ENV PYTHONUNBUFFERED=1 \
    PYTHONDONTWRITEBYTECODE=1 
#ENV LAB_LOGIN="${LAB_LOGIN}" \
#    LAB_TOKEN="${LAB_TOKEN}"

LABEL org.lab.login="${LAB_LOGIN}" \
      org.lab.token="${LAB_TOKEN}"

USER appuser

RUN mkdir -p /tmp/gunicorn && chown appuser:appgroup /tmp/gunicorn

HEALTHCHECK --interval=30s --timeout=10s --retries=3 \
    CMD curl -f http://localhost:8000/health || exit 1

STOPSIGNAL SIGTERM

#ENTRYPOINT ["python", "-m", "app.app"]
ENTRYPOINT ["gunicorn", "--bind", "localhost:8080", "app.app:app"]
