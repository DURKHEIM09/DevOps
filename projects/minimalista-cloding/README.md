## 🔁 Pós-Etapa 3 – Ações Complementares Realizadas

### 1️⃣ Testes Avançados da Aplicação no Cluster Kind

- Utilizamos `curl http://localhost:5001/health` com sucesso repetidas vezes.
- Garantimos que o `Deployment` e o `Service` estavam persistentes após aplicar os manifests.
- Logs confirmaram estabilidade e resposta correta da aplicação dentro do pod.

### 2️⃣ Verificação de Rede Interna e Estabilidade

- A aplicação Flask estava funcional e estável dentro do pod.
- Logs mostraram `Gunicorn running` e endpoints operando como esperado.

### 3️⃣ Ajustes Finos no Ambiente (Infraestrutura do Projeto)

- Consolidamos a nomenclatura dos diretórios: `app/`, `k8s/`, `infra/`, `ci/`, `docs/`.
- Iniciamos o alinhamento da Fase 5: **versionamento Git estruturado**.

### 4️⃣ Planejamento da Sequência

| Fase | Descrição                                     |
|------|-----------------------------------------------|
| 5️⃣   | Versionamento Git/GitHub com README estruturado |
| 6️⃣   | CI/CD com GitLab CI (.gitlab-ci.yml)          |
| 7️⃣   | IaC com Terraform                             |
| 8️⃣   | Observabilidade com Prometheus e Grafana      |

---

## ✅ Resumo Técnico

A Etapa 3 foi finalizada com sucesso e resultou em:

- Arquitetura do projeto consolidada com Kubernetes funcional.
- Validação completa de rede, logs e containers em ambiente Kind.
- Estrutura do repositório organizada para as próximas etapas: CI/CD, IaC e Observabilidade.
