# Episode 8: Proven Practices for Rolling Back

`web.yaml` is the starting point for the rollback demo. In the episode I make a change and then a typo, first undoing it the quick way with `kubectl rollout undo`, and we see why that leaves your file and your cluster out of sync. Then we fix it correctly by rolling back the file.

▶ Watch the episode: [Episode 08 - Rolling Back](https://youtu.be/fcJiJPu8KLg?si=UX0Is23yVtw07oUx)

## Try the demo yourself

```bash
# Set up: a folder with Git, and this file committed
mkdir newbernetes-ep8 && cd newbernetes-ep8
git init
cp /path/to/web.yaml .
git add web.yaml && git commit -m "web on nginx 1.27"

# 1. A correct upgrade, done the declarative way
kubectl apply -f web.yaml
sed -i 's/nginx:1.27/nginx:1.28/' web.yaml
git commit -am "nginx 1.28" && kubectl apply -f web.yaml
kubectl rollout history deployment/web

# 2. A typo pushed through the file
sed -i 's/nginx:1.28/nginx:no-such-tag/' web.yaml
git commit -am "bump nginx" && kubectl apply -f web.yaml

# 3. The quick fix
kubectl rollout undo deployment/web
kubectl diff -f web.yaml       # the file and the cluster disagree
kubectl apply -f web.yaml      # ...and the bad change comes right back

# 4. The declarative fix
git revert --no-edit HEAD
kubectl diff -f web.yaml       # preview the change
kubectl apply -f web.yaml
kubectl diff -f web.yaml       # no output means the file and cluster agree

# Clean up
kubectl delete -f web.yaml
```

Tip: if `git log` opens a scrolling view, press `q` to get out, or use `git --no-pager log --oneline`.
