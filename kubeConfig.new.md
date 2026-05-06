# Création d’un kubeconfig pour GitHub Actions avec k3s  
## (ServiceAccount + token non expirant)

Cette procédure permet de créer un kubeconfig stable (sans expiration de token)
afin de permettre à un pipeline GitHub Actions de déployer automatiquement
sur un cluster **k3s**, sans devoir modifier régulièrement les variables GitHub.

---

## 1. Création du ServiceAccount

Créer un ServiceAccount nommé `etudiant-srvaccount` dans les trois namespaces :

```bash
kubectl create serviceaccount etudiant-srvaccount -n dev
kubectl create serviceaccount etudiant-srvaccount -n qa
kubectl create serviceaccount etudiant-srvaccount -n prod
````

***

## 2. Création d’un Role avec permissions minimales

Créer le fichier `role-etudiant.yaml` :

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  name: etudiant-role
rules:
- apiGroups: ["", "apps"]
  resources: ["pods", "services", "deployments"]
  verbs: ["get", "list", "watch", "create", "update", "patch", "delete"]
```

Appliquer le rôle dans chaque namespace :

```bash
kubectl apply -f role-etudiant.yaml -n dev
kubectl apply -f role-etudiant.yaml -n qa
kubectl apply -f role-etudiant.yaml -n prod
```

***

## 3. Création des RoleBindings

Associer le ServiceAccount au rôle dans chaque namespace :

```bash
kubectl create rolebinding etudiant-rb \
  --role=etudiant-role \
  --serviceaccount=dev:etudiant-srvaccount \
  -n dev

kubectl create rolebinding etudiant-rb \
  --role=etudiant-role \
  --serviceaccount=qa:etudiant-srvaccount \
  -n qa

kubectl create rolebinding etudiant-rb \
  --role=etudiant-role \
  --serviceaccount=prod:etudiant-srvaccount \
  -n prod
```

Grâce à ces RoleBindings, **un seul token** pourra accéder aux trois namespaces.

***

## 4. Création d’un token non expirant (ServiceAccount token legacy)

> ⚠️ On **n’utilise pas** `kubectl create token`  
> Cette commande crée un token temporaire (1h), non adapté au CI/CD.

Créer le fichier `etudiant-token.yaml` dans le namespace `dev` :

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: etudiant-srvaccount-token
  namespace: dev
  annotations:
    kubernetes.io/service-account.name: etudiant-srvaccount
type: kubernetes.io/service-account-token
```

Appliquer le Secret :

```bash
kubectl apply -f etudiant-token.yaml
```

Après quelques secondes, Kubernetes génère automatiquement le token.

### Récupération du token

```bash
kubectl get secret etudiant-srvaccount-token -n dev \
  -o jsonpath='{.data.token}' | base64 -d
```

✅ Ce token **n’expire pas**  
✅ Il est adapté à une utilisation GitHub Actions

***

## 5. Récupération des informations du cluster

Afficher le kubeconfig maître de k3s :

```bash
sudo cat /etc/rancher/k3s/k3s.yaml
```

Récupérer la valeur suivante :

```yaml
certificate-authority-data: <BASE64_CA>
```

***

## 6. Construction du kubeconfig final

Créer le fichier `kubeconfig.yaml` :

```yaml
apiVersion: v1
kind: Config

clusters:
- name: k3s-cluster
  cluster:
    server: https://192.168.21.100:6443
    certificate-authority-data: <BASE64_CA>

users:
- name: etudiant
  user:
    token: <TOKEN_NON_EXPIRANT>

contexts:
- name: etudiant-context
  context:
    cluster: k3s-cluster
    user: etudiant
    namespace: dev

current-context: etudiant-context
```

***

## 7. Utilisation dans GitHub Actions

*   Stocker ce fichier (ou sa version base64) dans un **GitHub Secret**
*   L’injecter dans le workflow avant d’exécuter `kubectl` ou `helm`


