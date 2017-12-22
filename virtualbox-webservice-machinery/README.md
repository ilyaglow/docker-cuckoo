VirtualBox Web Service Machinery
--------------------------------

This folder is meant to hold customized `docker-compose.yml` and `Dockerfile` that represents [patched cuckoo version](https://github.com/cuckoosandbox/cuckoo/pull/1998) and few configuration files.

The manual assumes that Virtualbox 5+ is already installed on your host (Linux) and docker-cuckoo will run on the same host.

# How to set up Virtualbox Web Service

On the host you should create a user first that will run Virtualbox Web Service and will start/stop/import your VMs.

Ensure that a file `/etc/default/virtualbox` has a following format:

```
VBOXWEB_HOST=your-external-ip # IP reachable from cuckoo docker
VBOXWEB_USER=your-vbox-user   # created user
```

It is important to secure you Virtualbox Web Service with SSL using reverse proxy or built-in SSL option. The latter will require generating keyfile and appending `VBOXWEB_SSL_KEYFILE=path-to-crt` and `VBOXWEB_SSL_PASSWORDFILE=path-to-file-with-password` if needed to `/etc/default/virtualbox`.

Start Virtualbox Web Service:

```
systemctl start vboxweb
```

Do not forget to import certificate into Cuckoo docker container if you use self-signed certificate.

# How to set up this docker-compose

If you want to use this docker-compose and Virtualbox on the same host be sure that Virtualbox Host-only interface is created and your vboxnet0 IP address is `192.168.56.1`. If it differs, change a Cuckoo container `ports:` section accordingly.

Make a folder `/mnt/cuckoo-storage` (or any other one but change it in the docker-compose.yml then) on the host that will be used to store all cuckoo analysis data.

Change `./conf/virtualbox_websrv.conf` appropriately.

Run it:

```
docker-compose up -d
```
