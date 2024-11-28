# k8s deploy

- 1.k8s yaml 根据 helm 导出来的， 配置 openvpn.conf 和 ldap.conf 和 newClientCert.sh

- 2.生成客户端的脚本，注意命名空间

```sh
#!/bin/bash
POD_NAME=$(kubectl get pods -n "default"  -l "app=openvpn" -o jsonpath='{ .items[0].metadata.name }')
SERVICE_NAME=$(kubectl get svc -n "default" -l "app=openvpn" -o jsonpath='{ .items[0].metadata.name }')
SERVICE_IP=xxxxxxxx #具体看service如何暴露的
KEY_NAME=kubeVpn
kubectl --namespace "default" exec -it "$POD_NAME" /etc/openvpn/setup/newClientCert.sh "$KEY_NAME" "$SERVICE_IP"
kubectl --namespace "default" exec -it "$POD_NAME" cat "/etc/openvpn/certs/pki/$KEY_NAME.ovpn" > "$KEY_NAME.ovpn"
```
