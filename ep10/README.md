# Episode 10: Storage

Two small files that show how a PersistentVolumeClaim keeps your data when a pod is replaced.

▶ Watch the series: https://youtube.com/playlist?list=PLTVdODaaOJ90

- `claim.yaml` creates the `storage-demo` namespace and a 1Gi PersistentVolumeClaim called `notes-data`
- `app.yaml` runs a tiny busybox Deployment called `notes` with that claim mounted at `/data`

## Try it yourself

Apply the claim on its own first, and you'll see it sit in `Pending` until a pod needs it:

```bash
kubectl apply -f claim.yaml
kubectl get pvc -n storage-demo
```

Then add the app, and the claim changes to `Bound`:

```bash
kubectl apply -f app.yaml
kubectl get pvc -n storage-demo
```

Write a file to the volume, delete the pod, and check the file is still there in the new pod:

```bash
kubectl exec -n storage-demo deploy/notes -- sh -c 'echo "Hello from episode 10" > /data/hello.txt'
kubectl delete pod -n storage-demo -l app=notes
kubectl exec -n storage-demo deploy/notes -- cat /data/hello.txt
```

## Notes

- Built for K3s, which includes the `local-path` StorageClass. On another distribution, change `storageClassName` to a class your cluster has (`kubectl get storageclass` lists them).
- `local-path` stores each volume as a folder on one node, under `/var/lib/rancher/k3s/storage/`.
- Its reclaim policy is `Delete`, so cleaning up removes the data too:

```bash
kubectl delete -f app.yaml
kubectl delete -f claim.yaml
```
