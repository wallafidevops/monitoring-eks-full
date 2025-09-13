# Monitoring EKS Full

Este repositório contém a configuração completa para implantação de um stack de monitoramento em **Amazon EKS** utilizando **ArgoCD** e **Helm Charts**.  
A solução integra **Prometheus**, **Grafana**, **Loki** e **Promtail** para observabilidade e logging centralizado.

---

## 📌 Arquitetura

- **ArgoCD** → responsável pelo GitOps e sincronização dos manifests.
- **Prometheus (kube-prometheus-stack)** → coleta e armazena métricas do cluster.
- **Grafana** → dashboards para visualização das métricas.
- **Loki** → armazenamento centralizado de logs.
- **Promtail** → coleta de logs dos pods e envio para o Loki.

---

## 📂 Estrutura do Repositório

```
monitoring-eks-full/
│── argocd/                 # Manifests do ArgoCD (Applications)
│   ├── app-grafana.yaml
│   ├── app-loki.yaml
│   ├── app-promtail.yaml
│
│── kube-prometheus-stack/  # Chart do kube-prometheus-stack
│   ├── Chart.yaml
│   ├── values.yaml
│
│── promtail/               # Chart do Promtail
│   ├── Chart.yaml
│   ├── values.yaml
│
│── stack-loki/             # Chart do Loki
│   ├── Chart.yaml
│   ├── values.yaml
```

---

## 🚀 Pré-requisitos

- Kubernetes cluster (EKS)
- kubectl configurado
- Helm 3 instalado
- ArgoCD instalado no cluster  
  ```bash
  kubectl create namespace argocd
  kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
  ```

---

## ⚙️ Deploy com ArgoCD

1. Adicionar repositório no ArgoCD:
   ```bash
   argocd repo add https://github.com/wallafidevops/monitoring-eks-full.git
   ```

2. Criar as aplicações:
   ```bash
   kubectl apply -f argocd/app-grafana.yaml -n argocd
   kubectl apply -f argocd/app-loki.yaml -n argocd
   kubectl apply -f argocd/app-promtail.yaml -n argocd
   ```

3. Sincronizar no ArgoCD:
   ```bash
   argocd app sync grafana
   argocd app sync loki
   argocd app sync promtail
   ```

---

## 📊 Acesso aos Dashboards

- **Grafana**: exposto via Service/Ingress (conforme configurado no `values.yaml`)  
- **Prometheus**: acessível via `kube-prometheus-stack`  
- **Loki**: backend de logs integrado ao Grafana  
- **Promtail**: coleta e envio automático de logs dos pods  

---

## 🔧 Customização

Todos os ajustes de configuração podem ser feitos diretamente nos arquivos `values.yaml` de cada chart:  

- `kube-prometheus-stack/values.yaml` → métricas, regras de alerta, retention, etc.  
- `stack-loki/values.yaml` → storage e retention dos logs.  
- `promtail/values.yaml` → scraping de logs.  

---

## 📜 Licença

Este projeto é distribuído sob a licença **MIT**.  
Sinta-se livre para usar, modificar e contribuir.  

---

👉 [Repositório GitHub](https://github.com/wallafidevops/monitoring-eks-full)
