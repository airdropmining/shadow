# shadow


#### ssh
```shell
# login
$ ssh -i ~/.ssh/vultr-universal root@jp.airdropmining.com

# local to remote
$ cat v2ray/v2ray-443.service | ssh root@jp.airdropmining.com "cat >> /etc/v2ray/v2ray-443.service"
$ cat v2ray/config.json | ssh root@jp.airdropmining.com "cat >> /etc/v2ray/config.json"
$ scp -i -r ./v2ray/config.json root@jp.airdropmining.com:/etc/v2ray/config.json

# remote to local
$ tar -czvpf airdropmining.acme.tar.gz folderToCompress
$ scp root@jp.airdropmining.com:/etc/v2ray/airdropmining.acme.tar.gz ~/airdropmining.acme.tar.gz
$ sudo tar xpf airdropmining.acme.tar.gz

# restart
ssh -t root@jp.airdropmining.com "docker container restart v2ray-443"
```

#### acme
```shell
# issue certs
$ sudo ~/.acme.sh/acme.sh --issue --standalone -d airdropmining.com -d jp.airdropmining.com -d www.airdropmining.com --keylength ec-256 --force

# install certs
$ sudo ~/.acme.sh/acme.sh --installcert -d airdropmining.com --fullchainpath /etc/v2ray/airdropmining.com.crt --keypath /etc/v2ray/airdropmining.com.key --ecc

```

#### docker
```shell
$ docker run -d --name=v2ray-443 -v /etc/v2ray:/etc/v2ray -v /var/log/v2ray:/var/log/v2ray -p 443:443 v2ray/officialv2ray-443
$ docker container restart v2ray-443
$ docker container stop v2ray-443
$ docker container rm v2ray-443
$ docker container logs v2ray-443
```

#### service
```shell
$ mv -v v2ray-*.service /etc/systemd/system
$ sudo systemctl enable /etc/systemd/system/v2ray-*.service
$ sudo systemctl daemon-reload
$ sudo systemctl start v2ray-443

# remove
$ sudo systemctl stop v2ray-443
$ sudo systemctl disable v2ray-*.service

# moniter
$ systemctl status v2ray-443.service
$ journalctl -xn
$ journalctl -f -u v2ray-443.service

# Check the log in docker container with:
$ docker logs -t CONTAINER_ID

# Ensure the container v2ray-443 is running. Enter into it with:
$ docker exec -i -t v2ray-443 bash
```