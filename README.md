Docker image for digitalocean's CLI.

See https://github.com/digitalocean/doctl

## Usage
```
docker run -e DIGITALOCEAN_ACCESS_TOKEN=$DIGITALOCEAN_ACCESS_TOKEN farmenvy/doctl compute
```

## Creating a droplet

Use latest docker image: `docker-16-04`

You need access to an ssh key that is associated with your account.  Assuming you already uploaded one of your public keys to DigitalOcean, here's a handy way to get your fingerprint. Put this somewhere like `.zshrc`.

```
fingerprint() {
  pubkeyname="${1:-id_rsa.pub}"
  pubkeypath="${2:-$HOME/.ssh}"
  ssh-keygen -E md5 -lf "$pubkeypath/$pubkeyname" | awk '{ print $2 }' | cut -c 5-
}

# Usage:
# (assuming you have a public key ~/.ssh/digitalocean.pub)
#
# fingerprint digitalocean.pub
# => a1:b2:c3:d4:f5:g6:h7:i8:j9

```

```
docker run \
  -e DIGITALOCEAN_ACCESS_TOKEN=$DIGITALOCEAN_ACCESS_TOKEN \
  farmenvy/doctl compute \
  droplet create test \
  --size 1gb \
  --image docker-16-04 \
  --region nyc1 \
  --ssh-keys $(fingerprint digitalocean.pub)
```

## Destroying a droplet

First find the id of the droplet you want to delete

```
docker run \
  -e DIGITALOCEAN_ACCESS_TOKEN=$DIGITALOCEAN_ACCESS_TOKEN \
  farmenvy/doctl compute droplet list

```

Then delete:

```
docker run \
  -e DIGITALOCEAN_ACCESS_TOKEN=$DIGITALOCEAN_ACCESS_TOKEN \
  farmenvy/doctl compute droplet delete -f <droplet-id>

```
