# Proof of Concept: Розгортання ArgoCD в локальному Kubernetes кластері

## Вступ

На основі результатів попереднього аналізу, було обрано **k3d** як основний інструмент для локального розгортання Kubernetes кластеру.

Цей документ демонструє процес встановлення ArgoCD у кластер Kubernetes, розгорнутий за допомогою k3d.

---

## Встановлення k3d та створення кластеру

### Встановлення k3d

```bash
curl -s https://raw.githubusercontent.com/k3d-io/k3d/main/install.sh | bash
```

### Створення кластеру

```bash
k3d cluster create asciiartify-cluster
```

---

## Встановлення ArgoCD

### Створення простору імен

```bash
kubectl create namespace argocd
```

### Встановлення ArgoCD

```bash
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

---

## Доступ до графічного інтерфейсу ArgoCD

### Проксі до інтерфейсу

```bash
kubectl port-forward svc/argocd-server -n argocd 8080:443
```

Після цього відкрийте браузер і перейдіть за адресою:

```
https://localhost:8080
```

> Можливо буде попередження про безпечне з'єднання, оскільки використовується самопідписаний сертифікат.

### Отримання логіна та пароля

#### Логін:

```
admin
```

#### Пароль:

```bash
kubectl get secret argocd-initial-admin-secret -n argocd -o jsonpath="{.data.password}" | base64 -d
```

---

## Висновки

ArgoCD успішно встановлено у кластер Kubernetes, створений за допомогою k3d. Система доступна через вебінтерфейс, що дозволяє команді AsciiArtify легко реалізовувати підхід GitOps у своєму MVP.

## Корисні посилання

- [ArgoCD Getting Started](https://argo-cd.readthedocs.io/en/stable/getting_started/)