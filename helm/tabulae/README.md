# Tabulae Helm Chart

This chart deploys the Tabulae application on a local Kind Kubernetes cluster.

It includes the frontend, backend, PostgreSQL database, persistent storage, and Kubernetes health checks.

## Deploy

From the repository root:

```bash
helm lint helm/tabulae
helm upgrade --install tabulae helm/tabulae --wait --timeout 90s
kubectl get pods
kubectl get services
```

To access the frontend locally:

```bash
kubectl port-forward svc/tabulae-frontend 8080:80
```

Open http://localhost:8080.

## Rollback Test

I tested a Helm upgrade with an invalid frontend image. The upgrade failed, and Helm's `--atomic` option restored the previous working release. The application workloads and database storage were checked afterwards.

## Notes

This setup has been tested locally with Kind. The rollback test was manual; deployment through GitHub Actions and production deployment are not included yet.

Do not commit `.env` files or real credentials.
