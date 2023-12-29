# Install k8s-csi-s3 on Kubernetes Cluster
```bash
git remote add k8s-csi-s3 https://github.com/yandex-cloud/k8s-csi-s3.git
git subtree add --prefix k8s-csi-s3 k8s-csi-s3 tags/v0.40.0
```
- Add git subtree
- You dont need to execute above commands, it just show you how to setup

```bash
helm repo add yandex-s3 https://yandex-cloud.github.io/k8s-csi-s3/charts
helm install csi-s3 yandex-s3/csi-s3 \
    --namespace kube-system \
    --set secret.accessKey='<aws-accesskey>' \
    --set secret.secretKey='<aws-secertkey>' \
    --set secretendpoint='https://s3.<region>.amazonaws.com' \
    --set storageClass.singleBucket='<s3-bucketname>' \
    --set storageClass.mountOptions='-o allow_other --memory-limit 1000 --dir-mode 4777 --file-mode 4777'
```
- [k8s-csi-s3 reference](https://github.com/yandex-cloud/k8s-csi-s3/tree/v0.40.0?tab=readme-ov-file)
- s3 storage is cheaper than other cloud system and construct easiler than other platform

```bash
kubectl patch sc csi-s3 -p '{"metadata": {"annotations":{"storageclass.kubernetes.io/is-default-class":"true"}}}'
```
- Set default storage class

```bash
kubectl create -f k8s-csi-s3/deploy/kubernetes/examples/pvc.yaml
# After checking whether creating pvc or not, execute below command for deleting test pvc resource
kubectl delete -f k8s-csi-s3/deploy/kubernetes/examples/pvc.yaml
```
- Let's test whether making pvc or not

## local-path-provisioner
```bash
kubectl apply -f https://raw.githubusercontent.com/rancher/local-path-provisioner/v0.0.26/deploy/local-path-storage.yaml
```

# Install kubeflow on Kubernetes Cluster
```bash
git remote add kubeflow https://github.com/kubeflow/manifests.git
git subtree add --prefix kubeflow kubeflow v1.7-branch
```
- Add git subtree
- You dont need to execute above commands, it just show you how to setup

```bash
cd kubeflow
while ! kustomize build example | kubectl apply -f -; do echo "Retrying to apply resources"; sleep 10; done
```
- If you want to specify components in kubeflow, reference to ```kubeflow/example/kustomization.yaml```
- It just add '#' in component which you wouldn't install
- Kubernetes Version : v1.26
