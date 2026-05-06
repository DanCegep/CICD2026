## 1. Création d’un Personal Access Token (PAT)

### 1.1 Accéder aux paramètres GitHub

1. Se connecter à GitHub
2. Cliquer sur l’avatar (coin supérieur droit)
3. **Settings** Ou aller directement sur https://github.com/settings/profile
4. **Developer settings**
5. **Personal access tokens**
6. **Tokens (classic)**

> Les *tokens classic* sont suffisants et plus simples pour Git.

---

### 1.2 Générer un nouveau token

Cliquer sur **Generate new token (classic)**

#### Paramètres recommandés

- **Note** :
  ```text
  VS Code – Git – Accès dépôt
````

*   **Expiration** :
    *   `No expiration` (ou une durée longue)

*   **Scopes (permissions)** :
    *   ✅ `repo`

> Le scope `repo` permet :
>
> *   push
> *   pull
> *   gestion des branches
> *   accès aux dépôts privés

***

### 1.3 Sauvegarder le token

Après avoir cliqué sur **Generate token** :

⚠️ **Copier immédiatement le token**
(exemple : `ghp_xxxxxxxxxxxxxxxxxx`)

> GitHub ne permet **pas d’afficher à nouveau** ce token.

***

## 2. Utilisation du token pour pousser du code

### 2.1 Vérifier l’URL du dépôt (HTTPS)

```bash
git remote -v
```

Si nécessaire, corriger l’URL :

```bash
git remote set-url origin https://github.com/<UTILISATEUR>/<DEPOT>.git
```

***

### 2.2 Premier push avec authentification

```bash
git push origin main
```

Git demande alors :

```text
Username: <TON_UTILISATEUR_GITHUB>
Password: <TON_TOKEN_GITHUB>
```

➡️ **Le mot de passe est le PAT**, pas le mot de passe GitHub.

✅ Authentification réussie  
✅ Push effectué

***

## 3. Enregistrer le token (ne plus le retaper)

### Méthode recommandée : Git Credential Helper

#### Linux / macOS

```bash
git config --global credential.helper store
```

#### Windows

```bash
git config --global credential.helper manager
```

✅ Le token est stocké localement  
✅ Plus besoin de le retaper

***

### Vérification

```bash
git pull
```

➡️ Aucune demande d’identifiants → ✅ OK

***

## 4. Authentification dans VS Code (terminal)

### 4.1 Ouvrir le terminal VS Code

*   **Ctrl + \`**
*   ou **Terminal → New Terminal**

***

### 4.2 Configurer l’identité Git (une seule fois)

```bash
git config --global user.name "Ton Nom"
git config --global user.email "ton.email@exemple.com"
```

***

### 4.3 Travailler normalement

```bash
git status
git add .
git commit -m "Premier commit"
git push
```

✅ VS Code utilise la configuration Git globale  
✅ Aucune configuration supplémentaire requise

***

## 5. Option alternative (facultative) — GitHub CLI

Si GitHub CLI (`gh`) est installé :

```bash
gh auth login
```

Choisir :

*   GitHub.com
*   HTTPS
*   Coller le **PAT**

Vérification :

```bash
gh auth status
```

***
