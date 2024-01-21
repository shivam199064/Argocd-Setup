# Installation of argocd with helm
   #### helm install --dependency-update --namespace argocd  argocd argo-cd

## Port forward of argocd
   #### kubectl port-forward svc/argocd-server  8080:443 -n argocd >/dev/null &

## get initial initial password
   #### kubectl get secret argocd-initial-admin-secret -n argocd -o json | jq -r ".data.password"|base64 -d;echo


# Installation of argoworkflow with helm

## port forward of argo-workflow
   #### kubectl port-forward svc/argocd-argo-workflows-server  8081:2746 -n argo >/dev/null &

## find login token
   #### kubectl -n argo  exec -it deploy/argocd-argo-workflows-server -- argo auth token


# Create Kube-seal Secret

## create kube secret
   #### kubectl create secret generic secret-name   --from-literal=webhook=url

   ####  kubectl create secret generic argocd-notifications-secret --from-literal=webhook-url="https://url.com"  --dry-run=client -o yaml

   #### kubectl create secret generic argocd-notifications-secret --from-literal=webhook-url="https://url.com"  --dry-run=client -o yaml |kubeseal --controller-name="controller_name" --controller-namespace="namespace of controller" --namespace="In which namespace u want to create secret" -o yaml 

   If your controller name is : "sealed-secrets-controller", and your controller is in kube-system namespace
   ### kubectl create secret generic secretname --from-literal=webhook-url="https://url"  --dry-run=client -o yaml |kubeseal --namespace=argocd -o yaml


