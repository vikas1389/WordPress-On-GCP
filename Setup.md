##Commands ran in gshell:

1. Select a Project ID - default project gets created and selected when u open cloudshell on a newly created GCP account

2. Select Availibility Zone
   gcloud config set compute/zone us-west1-a
   
3. Enable API
   gcloud services enable container.googleapis.com sqladmin.googleapis.com
  
4. Create a wordpress cluster
   gcloud container clusters create wordpress --num-nodes=1
   
5. Configures kubectl and test it
   gcloud container clusters get-credentials wordpress
   kubectl get nodes
   
6. Create a deployment for wordpress
   kubectl apply -f WordPress-Deployment.yaml
   
7. Create a loadbalancer service to access wordpress externally
   kubectl apply -f WordPress-Service.yaml
   
8. Create a Horizontal Pod Autoscaler based on CPU utilization to handle the scalability.
   kubectl apply -f WordPress-HPA.yaml
   
9. See image WordPress-Screenshot.png that shows page comes up (http://34.82.53.117/wp-admin/install.php) and see the GKE output below for all resources

	enlightensyou@cloudshell:~ (future-champion-284923)$ kubectl get all
	NAME                             READY   STATUS    RESTARTS   AGE
	pod/wordpress-5c975cd9d-x7ccv    0/1     Pending   0          22m
	pod/wordpress-7fffdc48d6-fplgs   1/1     Running   0          39m
	NAME                        TYPE           CLUSTER-IP      EXTERNAL-IP    PORT(S)        AGE
	service/kubernetes          ClusterIP      10.59.240.1     <none>         443/TCP        109m
	service/wordpress-service   LoadBalancer   10.59.241.186   34.82.53.117   80:30963/TCP   29m
	NAME                        READY   UP-TO-DATE   AVAILABLE   AGE
	deployment.apps/wordpress   1/1     1            1           39m
	NAME                                   DESIRED   CURRENT   READY   AGE
	replicaset.apps/wordpress-5c975cd9d    1         1         0       22m
	replicaset.apps/wordpress-7fffdc48d6   1         1         1       39m
	NAME                                            REFERENCE              TARGETS   MINPODS   MAXPODS   REPLICAS   AGE
	horizontalpodautoscaler.autoscaling/wordpress   Deployment/wordpress   0%/50%    1         4         1          6m16s

   
10. Delete GKE cluster once demoed
   gcloud container clusters delete wordpress
   
   enlightensyou@cloudshell:~ (future-champion-284923)$ gcloud container clusters delete wordpress
	The following clusters will be deleted.
	 - [wordpress] in [us-west1-a]
	Do you want to continue (Y/n)?  Y
	Deleting cluster wordpress...done.
	Deleted [https://container.googleapis.com/v1/projects/future-champion-284923/zones/us-west1-a/clusters/wordpress].
   


##References:

	https://cloud.google.com/kubernetes-engine/docs/quickstart
	https://cloud.google.com/kubernetes-engine/docs/concepts/deployment
	https://cloud.google.com/kubernetes-engine/docs/how-to/horizontal-pod-autoscaling
	https://cloud.google.com/kubernetes-engine/docs/concepts/service


##Various ways to do WordPress-Deployment:

	https://cloud.google.com/wordpress


