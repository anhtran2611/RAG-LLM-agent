FROM python:3.10-slim AS build

RUN apt-get update
RUN apt-get -y install git && apt-get -y install curl

RUN python -m venv /opt/venv
ENV PATH="/opt/venv/bin:$PATH"

COPY requirements.txt .
RUN pip install --upgrade pip
RUN pip install -r requirements.txt

FROM python:3.10-slim

RUN apt-get update && apt-get install -y curl

COPY --from=build /opt/venv /opt/venv
ENV PATH="/opt/venv/bin:$PATH"
ENV OMP_NUM_THREADS=1

# Create necessary directories
RUN mkdir -p /rag-pipeline/src /data/models /data/vector_store

# Copy application code
COPY src/ /rag-pipeline/src/
COPY requirements.txt /rag-pipeline/

WORKDIR /rag-pipeline/src

EXPOSE 8000

CMD ["python", "main.py", "--port", "8000", "--model", "Qwen/Qwen2.5-0.5B-Instruct"]