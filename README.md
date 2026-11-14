# Episode 9: From theory to practice

`capstone.yaml` runs a real self-hosted app, [Uptime Kuma](https://github.com/louislam/uptime-kuma), next to the nginx web app we've used all series. Every piece of it maps back to an earlier episode, and the comments in the file say which one.

▶ Watch the episode: https://youtube.com/playlist?list=PLTVdODaaOJ90

## What it creates

Everything lives in one namespace, `newbernetes`:

- **web**: a Deployment of two nginx pods, with a Service in front
- **kuma**: Uptime Kuma, one replica, with its data on a PersistentVolumeClaim
- **An Ingress**: so you can open Kuma at `http://kuma.localhost` through Traefik, which K3s installs for you

## Try it yourself

```bash
kubectl apply -f capstone.yaml
kubectl get deploy,svc,ingress,pvc -n newbernetes
```

Then open `http://kuma.localhost`, set up an admin account (pick SQLite if it asks), and add an HTTP monitor for:

```
http://web.newbernetes.svc.cluster.local
```

If `kuma.localhost` doesn't load on your setup, use a port-forward instead and open `http://localhost:3001`:

```bash
kubectl port-forward -n newbernetes svc/kuma 3001:3001
```

## Notes

- Built for K3s. It uses K3s's built-in Traefik ingress and `local-path` storage. On another distribution you may need to change `ingressClassName` and `storageClassName`.
- Kuma runs as exactly one replica on purpose: its database is a single file on one disk. That's a preview of storage, where lesson 2 starts.
- Cleaning up deletes the namespace, the volume and Kuma's data:

```bash
kubectl delete -f capstone.yaml
```
