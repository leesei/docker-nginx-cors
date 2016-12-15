# docker-nginx-cors

A script for starting Docker's [`nginx:alpine`](https://github.com/nginxinc/docker-nginx/tree/master/mainline/alpine) image with CORS, and optionally HTTPS, on arbitrary folder and arbitrary port.

## Install

```sh
git clone https://github.com/leesei/docker-nginx-cors
```

## Usage

```sh
./nserve -p 9000 ~/share
```

Use this command to verify the config of the created container:
```sh
docker inspect --format="{{.Volumes}}" [${CONTAINER_NAME}|${CONTAINER_ID}]
docker inspect --format="{{.Config.Labels}}" [${CONTAINER_NAME}|${CONTAINER_ID}]

# with [`jq`](https://stedolan.github.io/jq/manual/)
docker inspect [${CONTAINER_NAME}|${CONTAINER_ID}] | jq .[0].Volumes
docker inspect [${CONTAINER_NAME}|${CONTAINER_ID}] | jq .[0].Config.Labels
```

You can also fork this repo and customize `nginx.conf`, `nginx.vh.default.conf` to fit your needs.

### HTTPS

- Place an certificate key pair in `conf.d/` (with name `https.{key,crt}`)
- Enable `https.conf` include in `conf.d/default.conf`

To create self-signed certs:
```sh
openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout nginx/conf.d/https.key -out nginx/conf.d/https.crt
# fill the IP/domain of your server to "Common Name"
# otherwise the browser will reject to connect to the server
```

## TODO

failed to show index (403)
