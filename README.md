# Installation of argocd with helm
   #### helm install --dependency-update --namespace argocd  argocd argo-cd

   ## Then create main Application
   ####  kubectl apply -f Application/main.yaml 

## Port forward of argocd
   #### kubectl port-forward svc/argocd-server  8080:443 -n argocd >/dev/null &

## get initial initial password
   #### kubectl get secret argocd-initial-admin-secret -n argocd -o json | jq -r ".data.password"|base64 -d;echo

# Create Kube-seal Secret and add it into extraobject of values.yaml file

## create kube secret
   #### kubectl create secret generic secret-name   --from-literal=webhook=url

   ####  kubectl create secret generic argocd-notifications-secret --from-literal=webhook-url="https://url.com"  --dry-run=client -o yaml

   #### kubectl create secret generic argocd-notifications-secret --from-literal=webhook-url="https://url.com"  --dry-run=client -o yaml |kubeseal --controller-name="controller_name" --controller-namespace="namespace of controller" --namespace="In which namespace u want to create secret" -o yaml 

   If your controller name is : "sealed-secrets-controller", and your controller is in kube-system namespace
   ### kubectl create secret generic secretname --from-literal=webhook-url="https://url"  --dry-run=client -o yaml |kubeseal --namespace=argocd -o yaml
Like that:
extraObjects: 
    
  - apiVersion: bitnami.com/v1alpha1
    kind: SealedSecret
    metadata:
      creationTimestamp: null
      name: argocd-notifications-secret
      namespace: argocd
    spec:
      encryptedData:
        webhook-url: AgBRBJBH5SKnbqwsz4N+V+FORmij7YjHZbdcAAEZ+OtNkhXosgDwDq7vhc/xDLK02EtGxg3rj2zCxcc+xh96aMlITy+bJrX7qILuXRl8nUsLewTAAZo5rnuSt24Y3tDuEvETw+FXaRXlfbcU749fNGii8lDQxZUBdZEILJKiM9VYB3osGlbP53vdua9yVbBXr0C9ASf1FPCtkYcMzbx6c/kWLAcNN8Mr9h9biafULadeon7y9qIz5Lfzda8KTTQjesxoWvKdmB0ojBP0/F5wzrwjt8fsjvdhaF7ea9wceNutS96Kwu+LuSQx1zzrwOP7ZC5L2/eaT9q3kHZYjWTNuTWi9lUzsw2tncTFm4cNiLsr1wh1mYmed4QrjJPOeHMJpeJZ6TL/mRawQsuJbKJexNBPXbUj9+FOg6voC0mD+tZRiT/SxN4Gj7PYAn8zmgvPqqB4fhaHA0VB62ASLaqREcoTumkBHAEAsPG6ampl2Ho81w0uvUiXTRRO8+Zi6nivgzd1u10iqQu+nHfC1h+pTYNHr5OdS+DKwhEJvE8hfOWfMOTIjN2eGpwH1PUOGIjBffYAtJiRQfuBqRPkE1pCX3OsW2J90uS3AgHchJ0bCFGKRLYImTZmkIVaxSgHYY8OU6UWZumU6otrW7xdcilP1tGMBl4oCFIeR7FtJVDtJIMHkz+dDyDNaUvlrO9coFSGXMIU8fjlAzStWqdZJh4YLDxK7krH93KA3gR1qmH5+KJlBYkk3g0Yd8xlAAnhlT+xXw74AtaASkcSGTtUMbdl4TIrzB+PlJSmFWK6dEdE/Q4CSdo4kNthUo44XtVLlP0NfpLkj8M9Hj0MY0HtqSnWM4GJeoYTX0SlFTmrPENB9mac8+K9IYMWOe08+h2orYzmhhQKXBD3FXpxVw==
      template:
        metadata:
          creationTimestamp: null
          name: argocd-notifications-secret
          namespace: argocd

          

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


# ArgoCD Image Updater

   ## play with cli
   #### kubectl -n argocd exec -it deploy/argocd-argocd-image-updater -- argocd-image-updater  version