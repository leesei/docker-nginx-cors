# docker-nginx-cors

A script for starting Docker's [`nginx:alpine`](https://github.com/nginxinc/docker-nginx/tree/master/mainline/alpine) image with CORS on arbitrary folder and arbitrary port.

## Install

```sh
git clone https://github.com/leesei/docker-nginx-cors
```

## Usage

```sh
./nserve -p 9000 ~/share
```

Use this command (requires [`jq`](https://stedolan.github.io/jq/manual/))to verify the config of the created container:
```sh
docker inspect [${CONTAINER_NAME}|${CONTAINER_ID}] | jq .[0].Volumes
docker inspect [${CONTAINER_NAME}|${CONTAINER_ID}] | jq .[0].Config.Labels
```

You can also fork this repo and customize `nginx.conf`, `nginx.vh.default.conf` to fit your needs.
