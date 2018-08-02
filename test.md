# TestDrive - PKS

## Preperation

Run the following command to get the PKS version and a list of available PKS CLI options<br>
> pks-h<br>


Login to the PKS environment using your PKS credentials from the Pivotal Container Service tile on the TestDrive Portal<br>

> pks login -a pks-api.vmwdemo.int -u `<username>` -p `<password>` -k<br>

Deploy a Kubernetes Cluster

> pks create-cluster `<username>`-1 -e `<username>`-1.vmwdemo.int --plan small<br>


Check the environment<br>

URL: [https://pks-vc-1.vmwdemo.int/vsphere-client](https://pks-nsxtmgr-1.vmwdemo.int)<br>
Username: pksdemo@vsphere.local<br>
Password: PKSdemo123!<br>

URL: [https://pks-nsxtmgr-1.vmwdemo.int](https://pks-nsxtmgr-1.vmwdemo.int)<br> 
Username: audit<br>
Password: PKSdemo123!<br>

Run the following command to list down all the k8s clusters created by your user

> pks cluster `<username>`-1 <br>

This command will populate the kube config file with the right credentials for the cluster <br>
> pks get-credentials `<username>`-1 <br>

List all the namespaces <br>
> kubectl get namespaces

List the PODs in all namespaces
> kubectl get pods --all-namespaces

Optional: enable K8S UI
> kubectl.exe proxy --port=0

In Google Chrome, navigate to 127.0.0.1:{port_number}/ui


# Deploy Restaurant Review Application

> cd C:\PKS\apps

> cat rest-review.yaml

Check out Harbor optionally

[harbor.vmwdemo.int/library/restreview-ui:V1]() which is the private Container Registry called Harbor. 


Creates all deployments and services defined in the yaml. 

> kubectl apply -f rest-review.yaml 

Get list of all the pods in the cluster

> kubectl get pods

View your deployment

> kubectl get deployments

View the number of replicas for this pod

> kubectl get rs

Get External LoadBalancer IP

> kubectl get svc

Open  Google Chrome; Enter the EXTERNAL-IP from the kubectl get svc. It should be in the format 192.168.x.x

# Delete Restaurant Review Application
Run the following commands to delete the application
> kubectl delete -f rest-review.yaml

