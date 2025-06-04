📦 Etapa 2 – Containerização da Aplicação com Docker
Nesta etapa, o foco é criar uma aplicação simples em Python Flask, containerizá-la usando Docker e testá-la localmente com Docker Compose.

🧱 Estrutura da Aplicação
bash
Copiar
Editar
/app
├── app.py
├── requirements.txt
├── Dockerfile
└── docker-compose.yml
🧰 Tecnologias Utilizadas
Python 3.11

Flask

Docker

Docker Compose v2

📜 Arquivos e Descrição
app.py
python
Copiar
Editar
from flask import Flask
app = Flask(__name__)

@app.route("/")
def home():
    return "🏠 Aplicação Flask DevOps OK!"

@app.route("/health")
def health():
    return "✅ STATUS: healthy"

if __name__ == "__main__":
    app.run(host="0.0.0.0", port=5000)
requirements.txt
nginx
Copiar
Editar
flask
Dockerfile
dockerfile
Copiar
Editar
FROM python:3.11-slim
WORKDIR /app
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt
COPY app.py .
EXPOSE 5000
CMD ["python", "app.py"]
docker-compose.yml
yaml
Copiar
Editar
services:
  web:
    build: .
    container_name: flask-app
    ports:
      - "5000:5000"
    restart: unless-stopped
🧪 Testes Locais
bash
Copiar
Editar
# Build da imagem
docker build -t flask-devops-demo .

# Execução com Docker direto
docker run -d -p 5000:5000 --name flask-app flask-devops-demo

# Ou com Compose
docker compose up -d

# Testes HTTP
curl http://localhost:5000        # 🏠 Aplicação Flask DevOps OK!
curl http://localhost:5000/health # ✅ STATUS: healthy
✅ Resultado da Etapa
Aplicação rodando localmente via container

Funcional e testável via HTTP

Base pronta para futura orquestração no Kubernetes

🗣️ Depoimento Pessoal (Lucas)
Nessa etapa eu tive alguns problemas, mas foram bem tranquilos de resolver. Um problema que eu tive foi referente à porta 5000 — já tinha um serviço anterior utilizando essa porta. Quando eu rodava a aplicação Flask, ela dava erro direto. Aí eu troquei a porta para 5001, atualizei no script e rodei de novo. Depois disso, subiu tranquilo.

Outro detalhe foi com permissão de arquivo. Eu fui executar um script e ele não tinha permissão de execução (+x), então deu erro. Aí eu só dei o chmod +x e rodei de novo, funcionou normal.

