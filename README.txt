# event management system — Ready Submission

## Files
- `Dockerfile.backend` — Spring Boot backend
- `Dockerfile.frontend` — React/Vite frontend (Nginx)
- `k8s-all.yaml` — MySQL + Backend + Frontend (Deployments + Services)

## Build & Push
Replace `<your-docker-username>` and run:

```bash
# From project root where folders frontend/ and backend/ exist

# Backend
docker build -f Dockerfile.backend -t <your-docker-username>/quiz-backend:latest .
docker push <your-docker-username>/quiz-backend:latest

# Frontend (API base points to backend service in cluster)
docker build -f Dockerfile.frontend --build-arg API_BASE=http://quiz-backend-service:9096 -t <your-docker-username>/quiz-frontend:latest .
docker push <your-docker-username>/quiz-frontend:latest
```

## Deploy to Kubernetes
```bash
kubectl apply -f k8s-all.yaml
kubectl get pods
kubectl get svc
```

Open the EXTERNAL-IP of `quiz-frontend-service` in your browser.

## Notes
- Backend listens on `9096` (set with `SERVER_PORT`).
- Database is MySQL with database `quiz` and root password from `mysql-secret`.
- Frontend build replaces `http://localhost:9096` with `http://quiz-backend-service:9096` automatically.
```
