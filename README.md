
<img src="https://avatars.githubusercontent.com/u/61287648" align="left" width="144px" height="144px"/>

> GitOps state for my cluster using flux v2

[![Discord](https://img.shields.io/badge/discord-chat-7289DA.svg?maxAge=60&style=flat-square)](https://discord.gg/DNCynrJ)
[![k8s](https://img.shields.io/badge/k8s-v1.26.3-orange?style=flat-square)](https://k8s.io/)
<!-- [![talos](https://img.shields.io/badge/talos-v1.4.0-yellow?style=flat-square)](https://k8s.io/) -->
[![GitHub last commit](https://img.shields.io/github/last-commit/fabricesemti80/flux-home-cluster?style=flat-square)](https://github.com/fabricesemti80/flux-home-cluster/commits/master)

# Post-deployment notes

(For the cluster deployment please see `DEPLOYMENT.md`)

## 🔧 Added tools

- [NFS External Provisioner](https://github.com/kubernetes-sigs/nfs-subdir-external-provisioner)
- [Cloudflared](https://github.com/cloudflare/helm-charts)
- [External DNS](https://github.com/kubernetes-sigs/external-dns)
- [Kyverno](https://github.com/kyverno/kyverno)
- [External secrets](https://github.com/external-secrets/external-secrets)
- [1Passowrd connect](https://github.com/1Password/connect-helm-charts)

## ☁️ Cloud Dependencies

While most of my infrastructure and workloads are selfhosted I do rely upon the cloud for certain key parts of my setup. This saves me from having to worry about two things. (1) Dealing with chicken/egg scenarios and (2) services I critically need whether my cluster is online or not.

The alternative solution to these two problems would be to host a Kubernetes cluster in the cloud and deploy applications like [HCVault](https://www.vaultproject.io/), [Vaultwarden](https://github.com/dani-garcia/vaultwarden), [ntfy](https://ntfy.sh/), and [Gatus](https://gatus.io/). However, maintaining another cluster and monitoring another group of workloads is a lot more time and effort than I am willing to put in.

| Service                                         | Use                                                               |
|-------------------------------------------------|-------------------------------------------------------------------|
| [1Password](https://1password.com/)             | Secrets with [External Secrets](https://external-secrets.io/)     |
| [Cloudflare](https://www.cloudflare.com/)       | Domain, DNS and proxy management

## 🙇 Inspirations / thanks

- <https://github.com/billimek/k8s-gitops>
- <https://github.com/haraldkoch/kochhaus-home>
- <https://github.com/onedr0p/home-ops>
- <https://github.com/nklmilojevic/home>                     |

## 📝 References / links

Password hashed with <https://bcrypt.online/> and stored in Bitwarden

[Test internal dns](https://help.hcltechsw.com/connections/v6/admin/install/cp_prereq_kubernetes_dns.html)
