# Episode 9: From theory to practice

`kumademo.yaml` runs a self-hosted app called [Uptime Kuma](https://github.com/louislam/uptime-kuma), along with the nginx web app we've used throughout the series. Every element maps back to an episode which you can read through in the comments.

▶ Watch the series: https://youtube.com/playlist?list=PLTVdODaaOJ90

## What it creates

Everything lives in a single namespace called `newbernetes`:

- **web**: a Deployment of two nginx pods within Service
- **kuma**: Uptime Kuma, one replica, with its data stored on a PersistentVolumeClaim
- **An Ingress**: so you can open Kuma at `http://kuma.localhost` through Traefik, which K3s installs for you

## Try it yourself

```bash
kubectl apply -f kumademo.yaml
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

- This was built for K3s. It uses K3s's built-in Traefik ingress and `local-path` storage. On another distribution you may need to change `ingressClassName` and `storageClassName`.
- Kuma runs as one replica because its database is a single file on one disk. This is actually a preview of the next set of episodes, which will deal with storage.
- Cleaning up deletes the namespace, the volume and Kuma's data:

```bash
kubectl delete -f kumademo.yaml
```
