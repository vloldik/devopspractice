*используется с каждой vm, далее <NO> - номер vm*

## 1. Установка etcd *скопировано с [оф. гитхаба](https://github.com/etcd-io/etcd/releases/tag/v3.5.14) и изменено*
```bash
ETCD_VER=v3.5.14

# choose either URL
GOOGLE_URL=https://storage.googleapis.com/etcd
GITHUB_URL=https://github.com/etcd-io/etcd/releases/download
DOWNLOAD_URL=${GOOGLE_URL}

mkdir /tmp/etcd-download
curl -L ${DOWNLOAD_URL}/${ETCD_VER}/etcd-${ETCD_VER}-linux-amd64.tar.gz -o /tmp/etcd-${ETCD_VER}-linux-amd64.tar.gz
tar xzvf /tmp/etcd-${ETCD_VER}-linux-amd64.tar.gz -C /tmp/etcd-download --strip-components=1
rm -f /tmp/etcd-${ETCD_VER}-linux-amd64.tar.gz

/tmp/etcd-download/etcd --version
/tmp/etcd-download/etcdctl version

cp /tmp/etcd-download/etcd /usr/bin/
cp /tmp/etcd-download/etcdctl /usr/bin/
rm -rf /tmp/etcd-download/
```

```
 etcd --name infra0 --initial-advertise-peer-urls http://192.168.0.124:2380 \
  --listen-peer-urls http://192.168.0.124:2380 \
  --listen-client-urls http://192.168.0.124:2379 \
  --advertise-client-urls http://192.168.0.124:2379 \
  --initial-cluster-token etcd-cluster-1 \
  --initial-cluster infra0=http://http://192.168.0.123:2380,infra1=http://192.168.0.124:2380,infra2=http://192.168.0.125:2380 \
  --initial-cluster-state new
```
