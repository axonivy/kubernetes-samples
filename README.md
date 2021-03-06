# Axon Ivy Kubernetes Samples

![Axon Ivy Engine on Kubernetes](axonivy-engine-on-kubernetes.png)

This repository shows you different [Kubernetes](https://kubernetes.io) setups with ivy.

All samples are based on the official [Axon Ivy Engine Docker Image](https://hub.docker.com/r/axonivy/axonivy-engine/).

## Run

Switch in a directory of your choice

    cd ivy

### Kubernetes

To run these examples you first have to run Kustomize:

    kubectl kustomize --load-restrictor="LoadRestrictionsNone" base > kubernetes.yaml

This gathers all the `*.yaml` setup files in the `base` directory and combines them into one single `kubernetes.yaml` file.
Create and start the kubernetes setup

    kubectl apply -f kubernetes.yaml

Afterwards the Axon Ivy Engine is available under http://localhost:8080 or via an Ingress under http://kubernetes.docker.internal/
Stop and delete the kubernetes setup

    kubectl delete -f kubernetes.yaml

#### Overlays

Some Kubernetes samples are prepared for additional environments or features. These samples have an `overlays` directory 
and one or multiple sub directories with the additional environment or features. To create a kubernetes.yaml for those 
environment or feature use the following command:

    kubectl kustomize --load-restrictor="LoadRestrictionsNone" overlays/xyz > kubernetes.yaml

If you have another type of environment or feature in your Kubernetes Cluster, for which no overlay is available, you might 
have to adjust the `*.yaml` files of the `base` directory accordingly.

#### NGNIX Ingress

Some samples are prepared to use a NGNIX [Ingress](https://kubernetes.io/docs/concepts/services-networking/ingress/). 
These samples have a `overlays/nginx-ingress` sub directory.
To run the samples with the Ingress you need to:
1. Install the NGNIX Ingress:

        kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.1.1/deploy/static/provider/cloud/deploy.yaml

2. Run kustomize with the overlays/nginx-ingress directory

        kubectl kustomize --load-restrictor="LoadRestrictionsNone" overlays/nginx-ingress > kubernetes.yaml
		
3. Create and start the kubernetes setup

        kubectl apply -f kubernetes.yaml

#### AWS Load Balancer Controller

Some samples are prepared to run on AWS with an AWS Load Balancer Controller installed on your Amazon EKS Kubernetes Cluster.
These samples have a `overlays/aws-load-balancer` sub directory.
To run the samples with the AWS Load Balancer Controller you need to:
1. Install AWS Load Balancer Controller. See [AWS documentation](https://docs.aws.amazon.com). 
2. Run kustomize with the overlay/aws-load-balancer

        kubectl kustomize --load-restrictor="LoadRestrictionsNone" overlays/aws-load-balancer > kubernetes.yaml
		
3. Create and start the kubernetes setup

        kubectl apply -f kubernetes.yaml		

## License

All these samples are shipped with a limited demo license. You are able to run the portal application.

## Docker Samples

Also have a look at our [docker samples](https://github.com/axonivy/docker-samples)