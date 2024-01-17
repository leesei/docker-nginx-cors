# docker-nginx-cors

A script for starting Docker's [`nginx:alpine`](https://github.com/nginxinc/docker-nginx/tree/master/mainline/alpine) image with CORS, and optionally HTTPS, on arbitrary folder and arbitrary port.

This is a script that load config files in the current directory rather than a Docker image. This allows for multiple instances of `docker-nginx-cors` running simultaneously with different configs.

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

### Access Control

- Create `conf.d/.htaccess` to specify the username and password  
  Or use Perl script, invoke multiple times for multiple users.

```sh
echo "<username>:`perl -le 'print crypt(\"<password>\", "salt-hash")'`:comment" >> nginx/conf.d/.htaccess
```

- Enable `auth-basic.conf` include in `conf.d/default.conf`
- You can also create new location block that includes `auth-basic.conf`

Its recommended to **enable HTTPS** with Basic Auth, otherwise the credentials will be sent in plaintext and vulnerable to sniffing.

## References

[h5bp/server-configs-nginx: Nginx HTTP server boilerplate configs](https://github.com/h5bp/server-configs-nginx)  
[Cipherli.st - Strong Ciphers for Apache, nginx and Lighttpd](https://cipherli.st/)  
[How To Set Up Nginx with HTTP/2 Support on Ubuntu 16.04 | DigitalOcean](https://www.digitalocean.com/community/tutorials/how-to-set-up-nginx-with-http-2-support-on-ubuntu-16-04)  
[HTTP 2.0 With Nginx - Servers for Hackers](https://serversforhackers.com/video/http-20-with-nginx)
[NGINX Docs | Restricting Access with HTTP Basic Authentication](https://docs.nginx.com/nginx/admin-guide/security-controls/configuring-http-basic-authentication/)
