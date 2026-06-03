Цей репозиторій містить GitOps-конфігурацію для розгортання повного моніторингового стеку для проєкту `kbot` у Kubernetes-кластері (середовище `dev`) за допомогою **Flux CD**.

## 🏗️ Архітектура рішення

* **Flux CD**: Забезпечує автоматичну синхронізацію стану кластера з цим репозиторієм (GitOps).
* **OpenTelemetry Operator + Collector**: Приймає інструментовані метрики та траси (із наскрізним TraceID) від `kbot`.
* **Prometheus**: Збирає метрики, згенеровані Otel Collector та системними компонентами.
* **Fluent-bit**: DaemonSet, який збирає логи з усіх нод кластера (`/var/log/containers/*.log`) та збагачує їх Kubernetes-метаданими.
* **Grafana Loki**: Централізоване сховище для логів.
* **Grafana**: Візуалізація метрик, логів та зв'язку між ними через TraceID.

---

## 🚀 Швидкий старт (Deployment)

1. Переконайтеся, що Flux CD встановлено у вашому кластері.
2. Сконфігуруйте доступ Flux до цього репозиторію:
   ```bash
   flux bootstrap github \
     --owner=$GITHUB_USER \
     --repository=kbot-monitoring \
     --branch=main \
     --path=clusters/dev \
     --personal