# ☸️ Etapa 3 – Deploy da Aplicação no Kubernetes (Kind)

Nesta etapa, a aplicação containerizada foi implantada em um cluster Kubernetes local utilizando Kind (Kubernetes-in-Docker). Foram utilizados manifests YAML para configurar o deployment e o service da aplicação.

---

## ⚙️ Ambiente Kubernetes

- Cluster local via Kind  
- Namespace: `default`  
- Exposição via `NodePort` (porta 30001)  
- Ferramentas utilizadas:  
  - `kubectl`  
  - `kind`  

---

## 📁 Arquivos Manifests

### `deployment.yaml`

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: flask-app
spec:
  replicas: 1
  selector:
    matchLabels:
      app: flask-app
  template:
    metadata:
      labels:
        app: flask-app
    spec:
      containers:
        - name: flask-container
          image: flask-devops-demo:latest
          imagePullPolicy: Never
          ports:
            - containerPort: 5000
```

### `service.yaml`

```yaml
apiVersion: v1
kind: Service
metadata:
  name: flask-service
spec:
  type: NodePort
  selector:
    app: flask-app
  ports:
    - port: 5000
      targetPort: 5000
      nodePort: 30001
```

---

## 🚀 Comandos Executados

```bash
kubectl apply -f deployment.yaml
kubectl apply -f service.yaml
kubectl get pods
kubectl logs -l app=flask-app
kubectl port-forward service/flask-service 5001:5000
curl http://localhost:5001/health
```

---

## ✅ Validação

- A aplicação respondeu corretamente nos endpoints `/` e `/health`  
- Logs confirmaram o funcionamento  
- A aplicação está acessível no cluster local  

---

## 🗣️ Depoimento Pessoal (Lucas)

Bom, nessa etapa o principal problema foi a questão da porta. Quando tentei acessar a aplicação via `curl` no localhost:5000, não funcionava porque a porta já estava sendo usada no host. A solução foi usar o `port-forward` com 5001:5000, aí funcionou normal.

Outro problema que eu enfrentei foi que o Kubernetes (Kind) **não reconhece a imagem local automaticamente**. Então mesmo a imagem `flask-devops-demo:latest` estando buildada, o cluster não encontrava. A solução foi usar o seguinte comando:

```bash
kind load docker-image flask-devops-demo:latest
```

Com isso o cluster passou a reconhecer a imagem normalmente e o deploy foi feito com sucesso. Nada muito complicado, mas são detalhes que fazem a diferença.

### 🧩 Problemas e Soluções

| Problema Técnico                      | Diagnóstico/Solução Aplicada                                           |
|--------------------------------------|------------------------------------------------------------------------|
| ❌ curl falhava no localhost:5000    | Porta já estava em uso no host                                        |
| ✅ Solução:                          | Usamos `kubectl port-forward` com `5001:5000`                         |
| ❌ Imagem não encontrada no cluster  | Kubernetes (Kind) não reconhece imagem local automaticamente           |
| ✅ Solução:                          | Comando `kind load docker-image flask-devops-demo:latest` foi usado   |
