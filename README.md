# geo-community-data-dfi

Données de filiation des parcelles (DFI) publiées sur GitHub Pages, pour les modules GEO : **un fichier JSON par
commune**, régénéré à chaque nouvelle situation de la DGFiP.

> Projet communautaire indépendant, **non affilié à Business Geografic**. GEO est un produit de Business Geografic.

## Adresse des données

`https://vienne-condrieu-agglomeration.github.io/geo-community-data-dfi/`

| Fichier | Contenu |
|---|---|
| `index.json` | version du format (`format`), source, licence, situation publiée (`situation`, `millesime`), départements, communes et nombre de lots |
| `<code INSEE>.json` | les lots de filiation d'une commune (ex. `38544.json`) |

Le format des fichiers est décrit dans le README de
[geo-community-integrator-dfi](https://github.com/vienne-condrieu-agglomeration/geo-community-integrator-dfi). Un module doit lire
`format` dans `index.json` avant tout et refuser une version qu'il ne connaît pas.

## Fonctionnement

Le workflow [`donnees.yml`](.github/workflows/donnees.yml) :

1. récupère l'`index.json` déjà publié ;
2. télécharge la dernière situation sur data.gouv et la convertit avec la version de l'intégrateur fixée dans le workflow
   (`INTEGRATOR_VERSION`) ;
3. ne publie rien si la situation, le périmètre et le format n'ont pas changé ;
4. sinon déploie le résultat sur GitHub Pages (sans commit : l'historique du dépôt ne grossit pas).

Il s'exécute chaque lundi et à la demande (*Actions > Données DFI > Run workflow*, avec en option une liste de départements
ou le forçage). **Aucun jeton n'est nécessaire** : le `GITHUB_TOKEN` automatique suffit.

Points d'attention :

- GitHub désactive les workflows planifiés d'un dépôt public sans activité depuis 60 jours. La DGFiP publie une fois par
  trimestre (janvier, avril, juillet, octobre) : après chacune, vérifier que la publication a eu lieu, sinon lancer le workflow à la main.
- Départements publiés : variable de dépôt `DFI_DEPARTMENTS` (*Settings > Secrets and variables > Actions > Variables*), par
  exemple `38 69` ; à défaut, `38 69`.
- Pour utiliser une nouvelle version de l'intégrateur, modifier `INTEGRATOR_VERSION` après avoir lu son `CHANGELOG.md`.
  Un changement de `format` régénère les données même si la situation est la même.

## Licences

- **Données** : issues du jeu de la DGFiP, [Licence Ouverte 2.0](https://www.etalab.gouv.fr/licence-ouverte-open-licence/).
  Mention à reprendre : « Source : DGFiP, Documents de filiation informatisés (DFI) des parcelles, situation de *mois année* ».
- **Workflow et documentation de ce dépôt** : [GPL-3.0-or-later](LICENSE) — © 2026 Vienne Condrieu Agglomération.

Les contributions au code de conversion se font sur
[geo-community-integrator-dfi](https://github.com/vienne-condrieu-agglomeration/geo-community-integrator-dfi) (signature DCO).
