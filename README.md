# AWX on Minikube (Arch)

## Prepare Environment

### Install components
pacman -S minikube docker docker-compose kubectl --noconfirm
systemctl start docker && systemctl enable docker

### Create values file (example)
cat <<EOF > values.yaml
AWX:
  enabled: false
  name: awx
  spec:
    admin_user: admin
  postgres:
    enabled: false
    host: Unset
    port: 5678
    dbName: Unset
    username: admin
    password: Unset
    sslmode: prefer
    type: unmanaged
EOF

## Install Operator

### Install Operator Helm Chart
helm repo add awx-operator https://ansible.github.io/awx-operator/
helm repo add
helm install -n awx --create-namespace awx-operator awx-operator/awx-operator -f values.yaml

### Access console
kubectl port-forward svc/awx-service -n awx 8080:80 &
kubectl get secret awx-admin-password -n awx -o yaml
echo "<BASE64_PASSWORD" | base64 -d

[Guide1](Helm install on existing cluster - Ansible AWX Operator Documentation)
[Guide2](https://github.com/ansible/awx-operator/blob/devel/.helm/starter/README.md)
