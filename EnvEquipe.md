# Connexion à l’environnement de travail d’équipe

Pour accéder à l’environnement de laboratoire, vous devez suivre les étapes ci-dessous.

---

## 1. Connexion avec OpenVPN

Vous devez d’abord vous connecter au réseau à l’aide de **OpenVPN**.

- Le logiciel **OpenVPN** est installé sur tous les ordinateurs des locaux d’informatique.
- Vous pouvez également l’installer sur votre ordinateur personnel afin de vous connecter à distance.

Pour vous connecter, vous aurez besoin :
- d’un **fichier de clé et de configuration**,
- d’un **nom d’utilisateur** et d’un **mot de passe**.

👉 Ces informations sont disponibles dans votre espace **Teams**, dans le devoir **« CICD d’équipe »**.

---

## 2. Configuration de votre connexion OpenVPN

Suivez les **3 étapes** ci-dessous pour configurer votre accès :

1. Ajouter un nouveau réseau
<br>
   <img width="294" height="505" alt="image" src="https://github.com/user-attachments/assets/d9e9fc20-3386-4b22-809f-07513e978777" />
<br>
<br>
2. Téléverser le fichier de clé et de configuration
<br>
<br>
    <img width="294" height="505" alt="image" src="https://github.com/user-attachments/assets/3a2bceb9-1c92-47c3-89e1-3b3884f803dd" />
<br>
<br>
3. Se connecter au réseau VPN
<br>
<br> 
    <img width="365" height="628" alt="image" src="https://github.com/user-attachments/assets/8a7c864e-803e-4d2e-81de-c653d740800a" />
<br><br>

Une fois la connexion établie, vous aurez accès au réseau interne du laboratoire.

---

## 3. Connexion à l’environnement Proxmox

Après vous être connecté via OpenVPN, accédez à l’environnement Proxmox à l’adresse suivante :

👉 **https://10.0.1.29:8006/**

- Vous disposerez d’un **compte d’équipe** pour vous connecter à Proxmox.
- Les informations de connexion se trouvent également dans le devoir **« CICD d’équipe »** sur Teams.

---

## 4. Machines virtuelles Kubernetes

Une fois connecté à Proxmox :

- Démarrez vos **deux machines virtuelles Kubernetes**.
- Une **troisième machine virtuelle** sera ajoutée prochainement pour le **runner GitHub Actions**.

---

## 5. Connexion aux machines virtuelles

Utilisez les informations suivantes pour vous connecter aux machines virtuelles :

```text
Utilisateur : admin4t4
Mot de passe : Passw0rd
