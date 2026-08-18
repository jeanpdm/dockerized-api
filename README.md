# dockerized-api

API REST simples de consulta de pedidos, **containerizada com Docker** e com manifests prontos para **Kubernetes**.

## Objetivo

Demonstrar empacotamento de uma aplicação em container, com boas práticas básicas: imagem enxuta, health check e configuração de orquestração.

## Endpoints

| Método | Rota              | Descrição                          |
|--------|-------------------|-------------------------------------|
| GET    | `/health`         | Health check (usado por k8s/docker) |
| GET    | `/pedidos/<codigo>` | Consulta um pedido pelo código    |
| POST   | `/pedidos`        | Cria um novo pedido                 |

## Como rodar com Docker

```bash
docker build -t pedidos-api .
docker run -p 5000:5000 pedidos-api
```

Ou com Docker Compose:

```bash
docker-compose up --build
```

Testar:

```bash
curl http://localhost:5000/health
curl http://localhost:5000/pedidos/PED-00001
```

## Como rodar com Kubernetes (opcional, via minikube/kind)

```bash
minikube start
eval $(minikube docker-env)
docker build -t pedidos-api:latest .
kubectl apply -f k8s/deployment.yaml
kubectl apply -f k8s/service.yaml
kubectl port-forward service/pedidos-api-service 5000:80
```

## Rodar os testes localmente (sem Docker)

```bash
pip install -r requirements.txt
pytest -v
```

## Tecnologias

- Python 3.12 + Flask
- Docker / Docker Compose
- Kubernetes (Deployment + Service)
- pytest
