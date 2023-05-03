# 🚀 CLOUDFLARED

## Development system setup

We use `brew` to install this.

```sh
brew install cloudflared

cloudflared login

# certificate saved into ~/.cloudflared/cert.pem
```

## Create tunnel

We create the tunnel, and observer this, along with - if there is any - our other tunnels.

```sh
cloudflared tunnel create k8s-tunnel

# tunnel secret is saved to #/.cloudflared/<tunel ID>.json.

cloudflared tunnel list
```

## Secure cloudflare tunnel

We add external Oauth to secure access to our app.

### cloudflare [gui] side

Start [here](https://one.dash.cloudflare.com/1443fe12026b33d56dcc26a9deed0667/home)

Set up a policy that allows access to the appllication. For this go to `Access > Applications >`

Then go to `Settings [left hand side] > Authentication > Add new > [Application name] > Configure` and add *all* the emails
(be aware: Github and Google can use different ones; use the *test* on the authentication, to find out what the authentication sends!) that needs to access the application(s)

### Identity provider side

- Github

Follow the instructions [here](https://one.dash.cloudflare.com/1443fe12026b33d56dcc26a9deed0667/settings/authentication/idp/add/github)

- Google

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

- also make sure the ingress of the app is prepared (you also need to have [external-dns](https://github.com/kubernetes-sigs/external-dns) for this to automatically create the dns records)

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
