# Meetings à la carte — version web

Site statique (un seul fichier `index.html`) + base Cloud Firestore (offre gratuite Spark).

## Accès
- À l'entrée, le site demande le **nom de l'entreprise** (et, en option, le nom de la personne).
- Le nom est normalisé : « Société Générale », « societe generale » et « SOCIETE-GENERALE » ouvrent le même espace.
- Toutes les personnes qui saisissent le même nom partagent les pôles, produits, cartes, jeux, demandes et le calendrier.
- Rôle (Developer / Manager) et produit : libres, sans contrôle. Protection faible, voulue.

## Données
`tenants/<nom-normalisé>/{poles, products, cards, requests, deck, members}` dans Firestore.
Aucun audit, aucun journal applicatif. On ne peut pas lister les entreprises depuis le site.

## Mise en place de Firebase (une fois, environ 10 minutes)
1. https://console.firebase.google.com → **Ajouter un projet** (Google Analytics : inutile).
2. **Build › Firestore Database › Créer une base** → mode **production** → emplacement en Europe (par ex. `eur3`).
3. Onglet **Règles** → remplacer par le contenu de `firestore.rules` → **Publier**.
4. **Paramètres du projet › Général › Vos applications › Web (</>)** → enregistrer l'application → copier l'objet `firebaseConfig`.
5. Coller cet objet dans `FIREBASE_CONFIG` en haut de `index.html` (ou le transmettre pour reconstruction).

Sans configuration, le site fonctionne en **mode démo** : les données restent dans le navigateur.

## Limites de l'offre gratuite (Spark, Cloud Firestore)
1 Gio stocké, 50 000 lectures et 20 000 écritures par jour, 10 Gio de transfert sortant par mois.
Une ouverture de page lit l'ensemble des documents de l'entreprise (environ 60 à 100) : ordre de grandeur, plusieurs centaines d'ouvertures par jour.

## Reconstruire
`python3 build_web.py [config.json]` depuis le dossier source produit `web/index.html`.
