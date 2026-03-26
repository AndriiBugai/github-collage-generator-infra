# GitHub Followers Collage Generator — Infra

Runs the [frontend](https://github.com/AndriiBugai/github-collage-generator-frontend) and [backend](https://github.com/AndriiBugai/github-collage-generator-backend) via Docker Compose or Kubernetes (minikube).

## Docker Compose

Create a `.env` file in the root of this repo:
```
GITHUB_TOKEN=your_github_token_here
```

Then run:
```bash
docker-compose up --build
```

App is available at `http://localhost:8081`.

---

## Kubernetes (minikube)

### Prerequisites

```bash
brew install minikube
minikube start
```

### 1. Point Docker to minikube's daemon

```bash
eval $(minikube docker-env)
```

> Run this in every new terminal session before building images.

### 2. Build images

```bash
docker build -t github-collage-generator-backend ../github-collage-generator-backend
docker build -t github-collage-generator-frontend ../github-collage-generator-frontend
```

### 3. Set your GitHub token

Edit `k8s/secret.example.yaml` and replace the placeholder with your token:
```yaml
stringData:
  github-token: your_github_token_here
```

> Do not commit this file with a real token.

### 4. Apply manifests

```bash
kubectl apply -f k8s/secret.yaml
kubectl apply -f k8s/backend.yaml
kubectl apply -f k8s/frontend.yaml
```

### 5. Wait for pods to be ready

```bash
kubectl get pods --watch
```

Both pods should reach `1/1 Running`. The backend takes ~30–60s to start.

### 6. Get the app URL

```bash
minikube service frontend --url
```

Open the printed URL in your browser.

### Teardown

```bash
kubectl delete -f k8s/
minikube stop
```
