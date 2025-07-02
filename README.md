# Web App Kubernetes Deployment

Пример отказоустойчивого и ресурсо-эффективного деплоймента веб-приложения в Kubernetes.

## Сценарий

- Мультизональный кластер (3 зоны, 5 нод)
- Приложение стартует за 5–10 секунд, требует бурст CPU на старте
- В пике справляется 4 пода, ночью нагрузка падает
- Потребление: 0.1 CPU в среднем, 128Mi памяти стабильно

## Файлы

- `deployment.yaml` — описание Deployment с `topologySpreadConstraints`
- `hpa.yaml` — HorizontalPodAutoscaler
- `pdb.yaml` — PodDisruptionBudget
- `README.md` — описание решений

## Принятые решения

- Используем 4 пода в пике, HPA уменьшает количество ночью до 1
- requests CPU выше, чем среднее потребление — из-за сложного старта
- Поддерживается отказоустойчивость по зонам и хостам
- PDB ограничивает количество одновременно удаляемых подов

## Развёртывание

```bash
kubectl apply -f deployment.yaml
kubectl apply -f hpa.yaml
kubectl apply -f pdb.yaml
```

