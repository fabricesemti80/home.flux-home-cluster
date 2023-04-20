# notes

## setting up cloudflared

## create tunnel

## configure cloudflare tunnel

## secure cloudflare tunnel

### cloudflare [gui] side

Start [here](https://one.dash.cloudflare.com/1443fe12026b33d56dcc26a9deed0667/home)

Set up a policy that allows access to the appllication. For this go to `Access > Applications > `

Then go to `Settings [left hand side] > Authentication > Add new > [Application name] > Configure` and add *all* the emails 
(be aware: Github and Google can use different ones; use the *test* on the authentication, to find out what the authentication sends!) that needs to access the application(s)

#### Google

### Identity provider side

#### Github

Follow the instructions [here](https://one.dash.cloudflare.com/1443fe12026b33d56dcc26a9deed0667/settings/authentication/idp/add/github)

#### Google

Follow the instructions [here](https://one.dash.cloudflare.com/1443fe12026b33d56dcc26a9deed0667/settings/authentication/idp/add/google)

