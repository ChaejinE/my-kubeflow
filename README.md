# Storage Class
## k8s-csi-s3
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

# Install kubeflow
```bash
cd kubeflow
while ! kustomize build example | kubectl apply -f -; do echo "Retrying to apply resources"; sleep 10; done
```
- Current latest version is v1.8.0