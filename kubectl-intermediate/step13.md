In this scenario, we are going to update the nginx image from nginx:1.16 to nginx:1.17
Before we proceed with the update, let's use  the same application in the previous step and scale it to 5 replicas again.

- So, let's scale the application

  `kubectl scale deployment/nginx-deployment --replicas=5`{{execute}}

-  We can verify whether the application has been scaled as fallow:
  
   `kubectl get pods`{{execute}} 

   `kubectl get deployment/nginx-deployment`{{execute}} 
   
   and check the Ready column 

-  Let's get all the pods name along with their current nginx image version:
   `kubectl get pods -o custom-columns=Pod_MAME:.metadata.name,IMAGE-VER:.spec.containers[*].image`{{execute}}

-  Now, let's proceed with the update. We will add the `--record` flag to capture and record the history of our rollout
  
   `kubectl set image deployment/nginx-deployment nginx=nginx:1.17 --record`{{execute}}
   
   Alternatively, you can achieve the same update result by editing the deployment manifest/config either manually or using the `kubeclt edit deployment DEPLOYMENT NAME` and nagigate to the .spec.template.spec.container[].image, change the image version and save.

   
   - We can watch rolling update status of `nginx-deployment` deployment until completion
     `kubectl rollout status -w deployment/nginx-deployment`{{execute}}

   - We can also get the rollout history:
     `kubectl rollout history deployment/nginx-deployment`{{execute}}

- Let's get again all the pods name along with their new nginx image version:
   `kubectl get pods -o custom-columns=Pod_MAME:.metadata.name,IMAGE-VER:.spec.containers[*].image`{{execute}}

-  To undo the update, run
   `kubectl rollout undo deployment/nginx-deployment`{{execute}}

   - to undo the update to a specific verion, pass the `--to-revision=` flag with the `kubectl rollout undo command` at the end.