# Setup gitlab and jenkins locally (kubernetes)
Kubernetes manifests for local deployment via Minikube, focusing on GitLab and Jenkins integration.

1. Prerequisites

Before you start, ensure you have the following software installed on your system:

    Git
    Docker
    Minikube (with kubectl)

On your terminal, clone this repo:

You can fork and clone the link of forked repo (**recommended**)

```bash
git clone git@github.com:EkberHasanov/setup-local-gitlab-jenkins-w-k8s.git src 
```
or
```bash
git clone git@github.com:<YOUR GITHUB USERNAME>/setup-local-gitlab-jenkins-w-k8s.git src
```
(**recommended**)
<br><br>

After cloning, you can check if there are proper files
```bash
ls src
```

### Let's start with initial configurations
In your terminal...
```bash
minikube start --driver docker
```
this code should start minikube

#### First start to setup and deploy gitlab
Execute these commands step by step
```bash
kubectl apply -f gitlab-deployment.yaml
```
then
```bash
kubectl apply -f gitlab-runner-deployment.yaml
```
And finally

```bash
kubectl apply -f gitlab-service.yaml
```

After executing these commands wait a little, then you can check if deployment went well visiting the local address of gitlab
For doing this you need to learn IP address of your minikube
You can learn it via this command

```bash
minikube ip
```

So visit the result of this command + NodePort of the gitlab-service
Let say resutl of the `minikube ip` command is <some-ip-address> and the NodePort is in the code:

```yaml
ports:
    - name: http
      protocol: TCP
      port: 80
      targetPort: 80
      nodePort: 30100
```
At result visit `<http://some-ip-address>:30100/`

#### It's turn of the jenkins
Execute these commands step by step
```bash
kubectl apply -f jenkins-persistentvolume.yaml
```

```bash
kubectl apply -f jenkins-persistentvolumeclaim.yaml
```

```bash
kubectl apply -f jenkins-deployment.yaml
```

And finally
```bash
kubectl apply -f jenkins-service.yaml
```

After waiting a while, you can visit its web interface like we did for gitlab above:
```yaml
      nodePort: 30200
```

```bash
http://<your-minikube-ip>:30200
```


## TODO
  * Create MakeFile
  * Create some bash scripts for automating some processes
