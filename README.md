
<img src="https://avatars.githubusercontent.com/u/61287648" align="left" width="144px" height="144px"/>

#### flux-home-cluster - Home Cloud via Flux v2 | GitOps Toolkit
> GitOps state for my cluster using flux v2

[![Discord](https://img.shields.io/badge/discord-chat-7289DA.svg?maxAge=60&style=flat-square)](https://discord.gg/DNCynrJ)
[![k8s](https://img.shields.io/badge/k8s-v1.26.3-orange?style=flat-square)](https://k8s.io/)
<!-- [![talos](https://img.shields.io/badge/talos-v1.4.0-yellow?style=flat-square)](https://k8s.io/) -->
[![GitHub last commit](https://img.shields.io/github/last-commit/fabricesemti80/flux-home-cluster?style=flat-square)](https://github.com/fabricesemti80/flux-home-cluster/commits/master)

<br />

# Post-deployment notes

(For the cluster deployment please see `DEPLOYMENT.md`)

## 🧑‍💻 USED APPLICATIONS/TOOLS

- [NFS External Provisioner](https://github.com/kubernetes-sigs/nfs-subdir-external-provisioner)

- [Cloudflared](https://github.com/cloudflare/helm-charts)


## 🚀 CLOUDFLARED

### Setting up cloudflared


We use `brew` to install this.

```sh
brew install cloudflared

cloudflared login

# certificate saved into ~/.cloudflared/cert.pem
```


### Create tunnel

We create the tunnel, and observer this, along with - if there is any - our other tunnels.

```sh
cloudflared tunnel create k8s-tunnel

# tunnel secret is saved to #/.cloudflared/<tunel ID>.json.

cloudflared tunnel list
```

### Secure cloudflare tunnel

We add external Oauth to secure access to our app.

#### cloudflare [gui] side

Start [here](https://one.dash.cloudflare.com/1443fe12026b33d56dcc26a9deed0667/home)

Set up a policy that allows access to the appllication. For this go to `Access > Applications > `

Then go to `Settings [left hand side] > Authentication > Add new > [Application name] > Configure` and add *all* the emails
(be aware: Github and Google can use different ones; use the *test* on the authentication, to find out what the authentication sends!) that needs to access the application(s)

#### Identity provider side


_Github_

Follow the instructions [here](https://one.dash.cloudflare.com/1443fe12026b33d56dcc26a9deed0667/settings/authentication/idp/add/github)

_Google_

Follow the instructions [here](https://one.dash.cloudflare.com/1443fe12026b33d56dcc26a9deed0667/settings/authentication/idp/add/google)

### Set up ingresses

The setting up of ingresses involves two things:

- create ingress rule (in `flux-home-cluster/kubernetes/apps/networking/cloudflared/app/helmrelease.yaml`)

```yaml

      ingress:

        - hostname: '*.${SECRET_DOMAIN}'
          service: https://ingress-nginx-controller.networking
          originRequest:
            noTLSVerify: true
```

this will re-direct all traffic for the `SECRET_DOMAIN` to the ingress controller (Nginx)

- also make sure the ingress of the app is prepared (you also need to have [external-dns]() for this to automatically create the dns records)

```yaml
    ingress:
      main:
        enabled: true
        ingressClassName: nginx
        annotations:
          # tunnel ->
          #? https://www.reddit.com/r/kubernetes/comments/z2vogg/cloudflare_and_ingressnginx/
          external-dns.alpha.kubernetes.io/target: <tunnel ID>.cfargotunnel.com
          external-dns.alpha.kubernetes.io/cloudflare-proxied: "true"
          # <- tunnel
```

On Cloudflare's website then you can set up a rule to authenticate acces, via `*. <YOURDOMAIN>.COM`

### Webhook acces

In order to let webhooks work, we need to set up a more specific rule:
`flux-webhook.<YOURDOMAIN>. com/hook/*` with authentication bypass.

## 🔒 OnePassword

- Install the [op cli](https://developer.1password.com/docs/cli/get-started/#install) following the instructions.
- [Connect to cli](https://developer.1password.com/docs/cli/get-started/)
- create secrets [guide](https://external-secrets.io/v0.5.7/provider-1password-automation/#deploy-a-connect-server)

```sh
## create vault
$ op vault create Kubernetes
# ID:                   gpuqypxjcegom3hypfbrstd744
# Name:                 Kubernetes
# Type:                 USER_CREATED
# Attribute version:    1
# Content version:      1
# Items:                0
# Created:              now
# Updated:              now

## create connect server
$ op connect server create Kubernetes --vaults Kubernetes
# File /home/fabrice/1password-credentials.json already exists, overwrite it? [Y/n] y
# Set up a Connect server.
# UUID: DN4A2BNR5BDQTB6UFS452RHSZ4
# Credentials file: /home/fabrice/1password-credentials.json

$  op connect token create Kubernetes --server Kubernetes --vaults Kubernetes
# < your token value>




```

## 🔧 Alterations / misc

## ☁️ Cloud Dependencies

While most of my infrastructure and workloads are selfhosted I do rely upon the cloud for certain key parts of my setup. This saves me from having to worry about two things. (1) Dealing with chicken/egg scenarios and (2) services I critically need whether my cluster is online or not.

The alternative solution to these two problems would be to host a Kubernetes cluster in the cloud and deploy applications like [HCVault](https://www.vaultproject.io/), [Vaultwarden](https://github.com/dani-garcia/vaultwarden), [ntfy](https://ntfy.sh/), and [Gatus](https://gatus.io/). However, maintaining another cluster and monitoring another group of workloads is a lot more time and effort than I am willing to put in.

| Service                                         | Use                                                               |
|-------------------------------------------------|-------------------------------------------------------------------|
| [1Password](https://1password.com/)             | Secrets with [External Secrets](https://external-secrets.io/)     |
| [Cloudflare](https://www.cloudflare.com/)       | Domain, DNS and proxy management                                  |

### GitOps

Password hashed with <https://bcrypt.online/> and stored in Bitwarden
