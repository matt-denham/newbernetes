# Newbernetes

Kubernetes for beginners, and this repo holds the YAML files from the demos in my YouTube series, [Newbernetes](https://youtube.com/playlist?list=PLTVdODaaOJ90), with one folder per episode.

Every demo runs on a single-node K3s cluster on openSUSE Leap, so feel free to follow along.

## Episodes

| Episode | Topic | Files |
|---|---|---|
| 1 | What is Kubernetes? | |
| 2 | Pods and Deployments | |
| 3 | Namespaces, Labels and Services | |
| 4 | Kubernetes Networking | |
| 5 | kubectl and YAML | [ep05](ep05) |
| 6 | Helm Charts | |
| 7 | Versions and Upgrades | |
| 8 | Rolling Back | [ep08](ep08) |
| 9 | From Theory to Practice | [ep09](ep09) |
| 10 | Storage | [ep10](ep10) |

Episodes without files used commands typed straight into the terminal, so there's nothing to download. Each folder's README lists the commands to try the demo yourself.

## What you need

- A Kubernetes cluster. I use [K3s](https://k3s.io), which installs with one command:

```bash
curl -sfL https://get.k3s.io | sh -
```

- `kubectl` (K3s includes it)

Some files rely on things K3s gives you out of the box, like the Traefik ingress controller and the `local-path` StorageClass. On another distribution, check the notes in that episode's README.

## Licence

Apache 2.0. See [LICENSE](LICENSE).
