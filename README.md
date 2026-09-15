# NDF CONNECT

## Démo de présentation — démarrage rapide

La page d’accueil est désormais une démo front-end autonome : aucune clé Supabase ni connexion n’est nécessaire.

```bash
git clone https://github.com/27032001/NDF_Connect.git
cd NDF_Connect
npm install
npm run dev:web
```

Ouvrir http://localhost:3000. Six vues sont disponibles : accueil, dépenses, coffre-fort, checklist, connexions et profil.
Le profil de présentation est Xavier Ogandaga. Le coffre-fort permet de créer des catégories, de les filtrer, d’ajouter des documents personnalisés et de reclasser les pièces existantes. Ces documents personnels ne modifient pas les exigences de la checklist.
Les interactions utilisent des données fictives en mémoire ; recharger la page réinitialise la démo.
L’ajout de dépense, la recherche, l’export CSV, l’ajout simulé de document et la mise à jour du score sont utilisables.
Les intégrations Google/Microsoft sont des aperçus. Les fichiers sélectionnés ne sont ni envoyés ni conservés.

Pour présenter : ouvrir l’accueil, ajouter une dépense, consulter le coffre-fort, puis compléter une pièce dans la checklist.

---

## Socle technique initial (hors parcours de présentation)

MVP mobile et web destiné aux étudiants en alternance, avec une attention particulière aux étudiants internationaux.

Ce dépôt contient le **Sprint 1** : monorepo TypeScript, authentification Supabase, profil utilisateur partagé et isolation des données par Row Level Security.

> NDF CONNECT ne remplace aucune administration et ne fournit pas d’avis juridique. Les futures règles administratives devront être configurées, datées et reliées à une source officielle. Toute règle non vérifiée sera explicitement signalée comme donnée de démonstration.

## Structure

```text
apps/mobile       Application Expo + React Native + Expo Router
apps/web          Application Next.js
packages/shared   Types, constantes et validations Zod partagés
supabase          Configuration locale, migrations et tests pgTAP
```

## Prérequis

- Node.js 20.19.4 ou version ultérieure ;
- npm 10 ou version ultérieure ;
- Docker Desktop pour Supabase local ;
- l’application Expo Go ou un simulateur Android/iOS pour le mobile.

## Installation

Depuis la racine du dépôt :

```bash
npm install
```

## Lancer Supabase en local

```bash
npm run supabase:start
npm run supabase:reset
```

La première commande affiche l’URL de l’API et la clé publique `anon`/`publishable`. Ne copiez jamais la clé `service_role` dans une application cliente.

Créez ensuite les fichiers d’environnement :

```text
apps/web/.env.local
apps/mobile/.env
```

Utilisez les modèles `apps/web/.env.local.example` et `apps/mobile/.env.example`, puis renseignez les valeurs retournées par Supabase.

Pour un téléphone physique, `127.0.0.1` désigne le téléphone lui-même. Remplacez cette adresse dans `apps/mobile/.env` par l’adresse IPv4 locale de l’ordinateur, par exemple `http://192.168.1.25:54321`, et autorisez le port dans le pare-feu si nécessaire.

Supabase Studio est disponible par défaut sur [http://127.0.0.1:54323](http://127.0.0.1:54323).

## Lancer le web

```bash
npm run dev:web
```

Ouvrez [http://localhost:3000](http://localhost:3000).

## Lancer le mobile

```bash
npm run dev:mobile
```

Scannez le QR code dans Expo Go ou choisissez un simulateur depuis le terminal Expo.

## Utiliser un projet Supabase hébergé

1. Créez un projet dans Supabase.
2. Associez le dépôt avec `npx supabase link --project-ref VOTRE_REFERENCE`.
3. Appliquez la migration avec `npx supabase db push`.
4. Copiez l’URL du projet et sa clé publique dans les fichiers d’environnement web et mobile.
5. Ajoutez les URL de redirection web et le schéma `ndfconnect://` dans la configuration Auth.

La migration crée automatiquement une ligne `profiles` lorsqu’un compte Auth est créé. Les politiques RLS autorisent uniquement la lecture et la modification du profil dont l’identifiant correspond à `auth.uid()`.

## Vérifications

```bash
npm run typecheck
npm run lint
npm run test
npm run build:web
```

Lorsque Supabase local est démarré :

```bash
npm run supabase:test
```

Le test pgTAP crée deux comptes temporaires et vérifie qu’un compte ne peut ni voir ni modifier le profil de l’autre. La transaction de test est annulée à la fin.

Pour exécuter toutes les vérifications TypeScript, lint et tests unitaires :

```bash
npm run verify
```

## Parcours livré au Sprint 1

- inscription par e-mail et mot de passe ;
- connexion, déconnexion et récupération de session ;
- session web renouvelée via cookies sécurisés ;
- session mobile persistée dans SecureStore ;
- route web et navigation mobile protégées ;
- consultation et modification du profil ;
- validation identique sur web et mobile ;
- profil automatiquement créé par trigger Supabase ;
- isolation RLS testée entre utilisateurs.

## Prochaine étape

Le Sprint 2 ajoutera les notes de frais, les catégories, les filtres mensuels et l’envoi privé des justificatifs.
