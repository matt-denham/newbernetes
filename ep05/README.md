# Episode 5: kubectl and YAML

`web.yaml` is the Deployment from the demo in episode 5. It runs three copies of nginx 1.27, and every Kubernetes object has the same four parts you can see in it: `apiVersion` and `kind`, `metadata`, `spec`, and the `status` the cluster writes back for you.

▶ Watch the episode: [Episode 5: kubectl and YAML](https://youtu.be/KlH3OYaEthI?si=ATSJ_C8XzZfSf_8r)

## Try the demo yourself

```bash
# 1. Let kubectl write the YAML for you (you'll get a file very close to web.yaml)
kubectl create deployment web --image=nginx:1.27 --dry-run=client -o yaml > my-web.yaml

# 2. Apply it, change replicas in the file to 3, preview the change, apply again
kubectl apply -f my-web.yaml
kubectl diff -f my-web.yaml
kubectl apply -f my-web.yaml

# 3. Break it on purpose with a tag that doesn't exist, then find out why
kubectl set image deployment/web nginx=nginx:no-such-tag
kubectl get pods
kubectl events --types=Warning

# 4. Let the file put everything back, then clean up
kubectl apply -f my-web.yaml
kubectl delete -f my-web.yaml
```

Or skip step 1 and use `web.yaml` from this folder directly.
