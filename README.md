# tinyHaproxy
A containerized Haproxy server using Alpine and Haproxy.

## Prerequisites packages
```
* buildah
* podman
```
## Configure container storage (Optional)
```bash
su - root
vgcreate vgDATA /dev/vdb
lvcreate -n lvLIB -L 5G /dev/vdb
lvcreate -n lvRUN -L 5G /dev/vdb
mkfs -t xfs -n ftype=1 /dev/vgDATA/lvLIB
mkfs -t xfs -n ftype=1 /dev/vgDATA/lvRUN
mount /dev/vgDATA/lvLIB /var/lib/containers
mount /dev/vgDATA/lvRUN /var/run/containers
```

## Usage
### Open firewall port
```bash
firewall-cmd --add-service=http --permanent
firewall-cmd --add-service=https --permanent
firewall-cmd --reload
```

### Clone git repository
```bash
git clone https:github.com/kevydotvinu/tinyHaproxy
cd tinyHaproxy
```

### Build container image
```bash
buildah bud --security-opt label=disable --tag localhost/kevydotvinu/tinyhaproxy:v1 .
```

### Run container image
```
podman run --rm \
           --interactive \
           --tty \
           --privileged \
           --net host \
           --volume "$(pwd)/haproxy.cfg:/etc/haproxy/haproxy.cfg" \
           --security-opt label=disable \
           --name tinyhaproxy localhost/kevydotvinu/tinyhaproxy:v1
```
To run the pod in background, replace --rm, --interactive and --tty with --detach.
