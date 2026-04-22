# 🏗️ BâtiConseil Pro — Site Vitrine + Panel Admin

Site vitrine pour cabinet de conseil en construction avec panel administrateur complet et synchronisation Firebase en temps réel.

---

## 📁 Structure du projet

```
/
├── index.html                  ← Site vitrine principal
├── assets/
│   ├── css/style.css           ← Styles du site vitrine
│   └── js/
│       ├── firebase-config.js  ← Config Firebase + gestionnaire de données
│       └── site.js             ← Moteur de rendu du site
├── admin/
│   ├── index.html              ← Panneau administrateur
│   ├── admin.css               ← Styles du panel admin
│   └── admin.js                ← Logique du panel admin
├── firebase.json               ← Config Firebase Hosting
└── .github/workflows/
    └── deploy.yml              ← Déploiement automatique GitHub Pages
```

---

## 🚀 ÉTAPE 1 — Créer le dépôt GitHub

1. Allez sur [github.com](https://github.com) → **New repository**
2. Nom du repo : `baticonseil-site` (ou votre choix)
3. Visibilité : **Public** (requis pour GitHub Pages gratuit)
4. Cliquez **Create repository**

Ensuite, uploadez tous les fichiers :
```bash
git init
git add .
git commit -m "Initial commit — BâtiConseil site"
git remote add origin https://github.com/VOTRE_USERNAME/baticonseil-site.git
git push -u origin main
```

---

## 📄 ÉTAPE 2 — Activer GitHub Pages

1. Dans votre repo GitHub → **Settings** → **Pages**
2. Source : **GitHub Actions**
3. Le workflow `.github/workflows/deploy.yml` déploiera automatiquement à chaque `git push`
4. Votre site sera disponible sur : `https://VOTRE_USERNAME.github.io/baticonseil-site/`

> ✅ Le panel admin sera accessible sur : `https://VOTRE_USERNAME.github.io/baticonseil-site/admin/`

---

## 🔥 ÉTAPE 3 — Configurer Firebase (synchronisation temps réel)

### 3.1 Créer le projet Firebase

1. Allez sur [console.firebase.google.com](https://console.firebase.google.com)
2. Cliquez **Ajouter un projet** → Nommez-le `baticonseil-site`
3. Désactivez Google Analytics si non nécessaire → **Créer le projet**

### 3.2 Activer Realtime Database

1. Dans le menu gauche → **Build** → **Realtime Database**
2. Cliquez **Créer une base de données**
3. Choisissez la région (europe-west1 recommandé)
4. Mode : **Démarrer en mode test** (pour commencer)
5. Règles à configurer plus tard pour la sécurité

### 3.3 Activer Storage (pour les images)

1. Dans le menu gauche → **Build** → **Storage**
2. Cliquez **Commencer**
3. Mode test → **Suivant** → Choisir la région → **Terminé**

### 3.4 Obtenir les identifiants Firebase

1. Dans Firebase Console → ⚙️ **Paramètres du projet** → **Général**
2. Faites défiler jusqu'à **Vos applications** → Cliquez **</>** (Web)
3. Nommez l'app `baticonseil-web` → **Enregistrer l'application**
4. Copiez le bloc `firebaseConfig`

### 3.5 Configurer le fichier firebase-config.js

Ouvrez `assets/js/firebase-config.js` et remplacez :

```javascript
const FIREBASE_CONFIG = {
  apiKey: "VOTRE_API_KEY",                    // ← Remplacer
  authDomain: "VOTRE_PROJECT.firebaseapp.com", // ← Remplacer
  databaseURL: "https://VOTRE_PROJECT-default-rtdb.firebaseio.com", // ← Remplacer
  projectId: "VOTRE_PROJECT_ID",              // ← Remplacer
  storageBucket: "VOTRE_PROJECT.appspot.com", // ← Remplacer
  messagingSenderId: "VOTRE_SENDER_ID",       // ← Remplacer
  appId: "VOTRE_APP_ID"                       // ← Remplacer
};
```

Par vos vraies valeurs Firebase.

---

## 🔒 ÉTAPE 4 — Sécuriser Firebase (IMPORTANT)

### Règles Realtime Database
Dans Firebase Console → Realtime Database → **Règles** :

```json
{
  "rules": {
    ".read": true,
    ".write": "auth != null"
  }
}
```

> Pour la production, activez l'authentification Firebase Auth et protégez l'écriture.

### Règles Storage
Dans Firebase Console → Storage → **Règles** :

```
rules_version = '2';
service firebase.storage {
  match /b/{bucket}/o {
    match /{allPaths=**} {
      allow read: if true;
      allow write: if request.auth != null;
    }
  }
}
```

---

## 🎛️ UTILISATION DU PANEL ADMIN

Accédez au panel admin via : `/admin/` (ou `admin/index.html`)

### Fonctionnalités disponibles :

| Section | Fonctionnalités |
|---------|----------------|
| 📊 Tableau de bord | Vue d'ensemble, actions rapides, statut Firebase |
| ⚙️ Paramètres | Nom du site, slogan, contact, logo, image hero |
| 🎨 Couleurs | Palette complète avec prévisualisation live |
| 🏷️ Catégories | Ajouter/modifier/supprimer avec icônes emoji |
| 🔧 Services | Gestion complète avec mise en avant |
| 📰 Articles | Éditeur avec brouillon/publié |
| ⭐ Témoignages | Gestion des avis clients |
| 🖼️ Galerie | Upload d'images avec drag & drop |
| 📈 Chiffres Clés | Modification du bandeau statistiques |
| 🔌 Extensions | Activer/désactiver les fonctionnalités |

---

## 🔄 Flux de synchronisation

```
Modification dans Admin Panel
         ↓
  Sauvegarde locale (localStorage)
         ↓
  [Si Firebase configuré] → Mise à jour Firebase Realtime DB
         ↓
  Firebase → Site vitrine (mise à jour instantanée)
```

Sans Firebase : Les données sont stockées dans le localStorage du navigateur et persistées entre sessions.

Avec Firebase : Toutes les modifications sont synchronisées en temps réel sur tous les appareils.

---

## 🛠️ Personnalisation avancée

### Changer les polices
Dans `assets/css/style.css`, modifiez l'import Google Fonts et les variables `font-family`.

### Ajouter une section personnalisée
1. Ajoutez une `<section id="ma-section">` dans `index.html`
2. Ajoutez le rendu dans `assets/js/site.js` dans la méthode `render()`
3. Ajoutez les styles dans `assets/css/style.css`

### Domaine personnalisé sur GitHub Pages
1. Achetez votre domaine (OVH, Gandi, etc.)
2. Dans GitHub → Settings → Pages → **Custom domain**
3. Chez votre registrar, ajoutez un enregistrement CNAME pointant vers `VOTRE_USERNAME.github.io`

---

## 📞 Support

Pour toute question sur la configuration Firebase ou le déploiement, consultez :
- [Documentation Firebase](https://firebase.google.com/docs)
- [Documentation GitHub Pages](https://docs.github.com/en/pages)
