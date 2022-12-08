https://source.cloud.google.com/ycit019-349515/x008930-notepad/+/master:backstage-project/

export PROJECT_ID=backstage-project-367900
gcloud config set project $PROJECT_ID
gcloud services enable compute.googleapis.com cloudresourcemanager.googleapis.com container.googleapis.com servicenetworking.googleapis.com


gcloud container clusters get-credentials backstage-project-cluster --region us-central1 --project backstage-project-367900



git clone https://github.com/backstage/backstage.git


//Creat namespace
kubectl apply -f namespace.yaml

//Postgres deploy
kubectl apply -f postgres-secrets.yaml
kubectl apply -f postgres-storage.yaml
kubectl apply -f postgres.yaml
kubectl apply -f postgres-service.yaml


//Backstage deploy
kubectl apply -f backstage-secrets.yaml
kubectl apply -f backstage.yaml
kubectl apply -f backstage-service.yaml


//Create the load balancer to expose the application
kubectl expose deployment backstage --type=LoadBalancer --name=lb-service




//clean up and delete

kubectl delete service lb-service

kubectl delete -f backstage.yaml
kubectl delete -f backstage-service.yaml

kubectl delete pvc data-backstage-postgresql-0
kubectl delete secret backstage-postgresql-certs
kubectl delete configMap backstage-postgres-ca
kubectl delete -f namespace.yaml
