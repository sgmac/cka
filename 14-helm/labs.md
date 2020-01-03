# Helm v3


```
wget https://get.helm.sh/helm-v3.0.0-linux-amd64.tar.gz
tar -xvf helm-v3.0.0-linux-amd64.tar.gz
sudo cp linux-amd64/helm  /usr/local/bin/helm3
helm3 --debug install firstdb stable/mariadb --set master.persistence.enable=false --set slave.persistence.enabled=false
helm3 repo update
$ helm3 search hub database
helm3 search hub database
helm3 repo add stable https://kubernetes-charts.storage.googleapis.com
helm3 --debug install firstdb stable/mariadb --set master.persistence.enabled=false --set slave.persistence.enabled=false
kubectl get secret --namespace default firstdb-mariadb -o jsonpath="{.data.mariadb-root-password}" | base64 --decode
helm3 list
helm3 uninsall firstdb
helm3 list
ls ~/.cache/helm/repository/
tar -zxvf mariadb-7.3.1.tgz
cp mariadb/values.yaml  ~/custom.yaml
vim ~/custom.yaml
helm3 install -f  ~/custom.yaml  stable/mariadb --generate-name
kubectl run mariadb-1578088061-client --rm --tty -i --restart='Never' --image  docker.io/bitnami/mariadb:10.3.21-debian-9-r --namespace default --command -- bash
helm list
helm3 list
helm3
history
helm3 list
helm3 uninstall mariadb-1578088061t
```
