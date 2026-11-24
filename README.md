# Gestion location moyenne et longue durée — Lot 1
 
Contenu du lot 1 : connexion et rôles, validation des comptes, réglages société,
fiches véhicules, catégories, segments de durée, grilles tarifaires (avec prix
programmés à une date) et simulateur de prix.

Technique : React + Vite, base et comptes Supabase, hébergement Cloudflare Pages.

## Installation (une seule fois)

### 1. Supabase
1. Crée un projet gratuit, séparé de Mobybuy, en région Europe.
2. SQL Editor → New query → colle `supabase/location_schema_v1.sql` → Run.
   (Si tu avais déjà lancé une version précédente du script, lance aussi
   `supabase/correctif_01_droits_profils.sql`.)
3. Project Settings → API : copie **Project URL** et la clé **anon public**.
   Ne copie jamais la clé `service_role` dans l'appli ou dans GitHub.

### 2. GitHub
1. Crée un dépôt **privé** (ex. `location-app`).
2. Add file → Upload files → glisse **le contenu** du dossier `location-app`
   (pas le dossier lui-même : `package.json` doit être à la racine du dépôt).
3. Commit changes.

### 3. Cloudflare Pages
1. Workers & Pages → Create → Pages → Connect to Git → choisis le dépôt.
2. Framework preset : **Vite** (sinon : build command `npm run build`,
   build output directory `dist`).
3. Variables d'environnement (Production) :
   - `VITE_SUPABASE_URL` = l'URL du projet Supabase
   - `VITE_SUPABASE_ANON_KEY` = la clé anon
4. Save and Deploy. Note l'adresse obtenue (ex. `location-app.pages.dev`).

### 4. Relier Supabase à l'adresse de l'appli
Supabase → Authentication → URL Configuration :
- **Site URL** = l'adresse Cloudflare (ex. `https://location-app.pages.dev`)
- **Redirect URLs** : ajoute la même adresse.
Sans ça, les liens de confirmation d'e-mail renvoient vers `localhost`.

### 5. Devenir administrateur
1. Ouvre l'appli → Créer un compte → confirme l'e-mail → connecte-toi.
   L'écran « Compte en attente de validation » s'affiche : c'est normal.
2. Supabase → SQL Editor :
   ```sql
   update profils set role = 'admin', actif = true where email = 'ton.email@domaine.fr';
   ```
3. Dans l'appli, clique « Vérifier à nouveau ».
Ensuite, tes collaborateurs s'inscrivent et tu les valides dans Réglages → Utilisateurs.

## Premiers réglages dans l'appli
Le tableau de bord affiche une liste « Mise en route ». Dans l'ordre :
1. Réglages → Société (raison sociale, logo, SIRET, IBAN).
2. Catégories (ex. Citadine, Compacte, SUV, Utilitaire).
3. Segments de durée (6 segments déjà créés, modifiables).
4. Grilles tarifaires : clique sur chaque case « À définir ».
5. Véhicules.

## Mettre à jour l'appli plus tard
Remplace les fichiers modifiés dans GitHub (Upload files) : Cloudflare
redéploie tout seul en 1 à 2 minutes.

## En cas d'erreur de compilation sur Cloudflare
Ouvre le déploiement en échec → copie les dernières lignes du journal de
build et envoie-les : la correction est en général rapide.
