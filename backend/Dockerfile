# DGX Spark / GB10 — Blackwell (sm_121), ARM64. NGC PyTorch supplies the only
# CUDA-enabled aarch64 torch for this GPU. Never pip-install torch over it.
FROM nvcr.io/nvidia/pytorch:25.11-py3

ENV DEBIAN_FRONTEND=noninteractive
WORKDIR /app

RUN apt-get update && apt-get install -y --no-install-recommends \
    ffmpeg libsm6 libxext6 libxrender1 libgl1 libgomp1 git wget curl \
    && rm -rf /var/lib/apt/lists/*

COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

COPY . .
RUN mkdir -p /tmp/avatars /tmp/videos /tmp/audio voice_profiles
COPY entrypoint.sh .
RUN chmod +x entrypoint.sh
EXPOSE 8000
HEALTHCHECK --interval=30s --timeout=10s --start-period=120s --retries=5 \
    CMD python -c "import urllib.request; urllib.request.urlopen('http://localhost:8000/health')"
CMD ["./entrypoint.sh"]
