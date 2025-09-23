
# 📘 Kubernetes – užitečné příkazy

## 🔹 Získání přehledu
```bash
kubectl get pods                 # seznam podů
kubectl get services             # seznam služeb
kubectl get deployments          # seznam deploymentů
kubectl get nodes                # seznam nodeů
````

## 🔹 Detailní informace

```bash
kubectl describe pod <pod-name>  # detailní info o podu
kubectl logs <pod-name>          # logy konkrétního podu
kubectl exec -it <pod-name> -- sh  # přihlášení do kontejneru
```

## 🔹 Nasazení a správa

```bash
kubectl apply -f file.yaml       # vytvoření nebo update z manifestu
kubectl delete -f file.yaml      # smazání podle manifestu
kubectl rollout restart deployment <name>  # restart deploymentu
kubectl rollout undo deployment <name>     # rollback na předchozí verzi
```

## 🔹 Kontexty a namespace

```bash
kubectl config get-contexts      # zobrazí všechny kontexty
kubectl config use-context <ctx> # přepnutí kontextu
kubectl get pods -n <namespace>  # seznam podů v konkrétním namespace
```

