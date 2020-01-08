# Helm chart to install team2-kubeop operator

[kubeop-operator](https://github.com/sreeragsreenath/team2-kubeop) is a Kubernetes operator.

## Introduction

This chart bootstraps a kubeop operator deployment on a kubernetes cluster using the [Helm](https://helm.sh) package manager.

## Prerequisites

- Kubernetes 1.16+
- IAM user with following policies: 
```bash
- IAMFullAccess (arn:aws:iam::aws:policy/IAMFullAccess) 
- AmazonS3FullAccess (arn:aws:iam::aws:policy/AmazonS3FullAccess)
```
Create a mysecrets.yaml file in templates/ with the following format:

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: iam-secret
  namespace: {{ .Values.operator.namespace }}
type: Opaque
data:
    AWS_ACCESS_KEY_ID: "BASE64-Encoded-AWSAccessKey"
    AWS_SECRET_ACCESS_KEY: "BASE64-Encoded-AWSSecreetAccessKey"
    BUCKET_NAME: "BASE64-Encoded-BucketName"
```
To encode your IAM user credentials to base64, use the following python code snippet:

 ```bash
$ python3
>>> import base64
>>> base64.b64encode(b'your string')
 ```

## go to the operator folder and run the following commands	
```bash	
operator-sdk generate k8s && operator-sdk generate openapi

operator-sdk build {{docker username}}/team2-kubeop:latest

docker push {{docker username}}/team2-kubeop:latest
```

## Installing the Chart

To install the chart:

```console
helm install --name=my-release s3folder-kubeop-chart --set image.repository={{docker username}}/team2-kubeop --set image.tag=latest
```

The command deploys kubeop-operator on the Kubernetes cluster in the default configuration. The [configuration](#configuration) section lists the parameters that can be configured during installation.

> **Tip**: List all releases using `helm list`

## Uninstalling the Chart

To uninstall/delete the `my-release` deployment:

```console
$ helm delete my-release
```
The command removes all the Kubernetes components associated with the chart and deletes the release.

## Configuration

The following table lists the configurable parameters of the kubeop operator chart and their default values.

| Parameter               | Description                                                                                                 | Default                         |
| :---------------------- | :---------------------------------------------------------------------------------------------------------- | :------------------------------ |
| `image.repository`      | Controller container image repository                                                                       | `{docker_username}/team2-kubeop` |
| `image.tag`      | Controller container image tag                                                                                     | `/latest` |
| `image.pullPolicy`      | Controller container image pull policy                                                                      | `IfNotPresent`                  |
| `serviceAccount.name`      | Service account                                                                                          | `team2-kubeop` |
| `operator.namespace`             | Namespace to install the operator                                                                  | `kubeop`                        |


## Developer information

| Name | NEU ID | Email Address | Github username |
| --- | --- | --- | --- |
| Jai Soni| 001822913|soni.j@husky.neu.edu | jai-soni |
| Riddhi Kakadiya| 001811354 | kamlesh.r@husky.neu.edu | riddhiKakadiya |
| Sreerag Mandakathil Sreenath| 001838559| mandakathil.s@husky.neu.edu| sreeragsreenath |
| Vivek Dalal| 001430934 | dalal.vi@husky.neu.edu | vivdalal |

Your feedback is always welcome!
