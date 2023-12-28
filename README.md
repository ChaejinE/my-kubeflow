# Install k8s-csi-s3 on Kubernetes Cluster
```bash
git remote add k8s-csi-s3 https://github.com/yandex-cloud/k8s-csi-s3.git
git subtree add --prefix k8s-csi-s3 k8s-csi-s3 tags/v0.40.0
```
- add git subtree

```bash
helm repo add yandex-s3 https://yandex-cloud.github.io/k8s-csi-s3/charts
helm install csi-s3 yandex-s3/csi-s3 \
    --namespace kube-system \
    --set accessKey=<aws-accesskey> \
    --set secretKey=<aws-secertkey> \
    --set endpoint='https://s3.<region>.amazonaws.com' \
    --set singleBucket=<s3-bucketname>
```
- [k8s-csi-s3 reference](https://github.com/yandex-cloud/k8s-csi-s3/tree/v0.40.0?tab=readme-ov-file)
- s3 storage is cheaper than other cloud system and construct easiler than other platform

```bash
kubectl patch sc csi-s3 -p '{"metadata": {"annotations":{"storageclass.kubernetes.io/is-default-class":"true"}}}'
```
- Set default storage class

```bash
kubectl create -f examples/pvc.yaml
```
- Let's test whether making pvc or not

# Install kubeflow on Kubernetes Cluster
```bash
git remote add kubeflow https://github.com/kubeflow/manifests.git
git subtree add --prefix kubeflow kubeflow v1.8-branch
```
- add git subtree

```bash
cd kubeflow
while ! kustomize build example | kubectl apply -f -; do echo "Retrying to apply resources"; sleep 10; done
```
- If you want to specify components in kubeflow, reference to ```kubeflow/example/kustomization.yaml```
- It just add '#' in component which you wouldn't install
- Kubernetes Version : v1.26
