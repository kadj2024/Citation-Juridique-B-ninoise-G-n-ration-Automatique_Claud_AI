---
name: citation-juridique-beninoise
description: >
  Génère automatiquement des notes de bas de page et des références bibliographiques
  complètes selon les normes académiques de l'ED-SJPA (École Doctorale des Sciences
  Juridiques, Politiques et Administratives) de l'Université d'Abomey-Calavi (UAC),
  Bénin. Utiliser ce skill dès que l'utilisateur fournit des données bibliographiques
  (auteur, titre, année, éditeur, URL, DOI, etc.) et demande à générer des citations,
  des références, des notes de bas de page, une bibliographie, ou un rapport de 
  références. Également déclencher pour toute demande de "mise en forme bibliographique",
  "formatter mes références", "générer ma bibliographie", "créer les notes de bas de page",
  ou toute demande impliquant des citations juridiques académiques béninoises/françaises.
  Produit en sortie un rapport Word (.docx) structuré avec deux sections distinctes :
  notes de bas de page et références bibliographiques.
---

# Citation Juridique Béninoise — Génération Automatique

Ce skill génère des notes de bas de page et des références bibliographiques selon
les normes **ED-SJPA / UAC**, telles que définies dans le fascicule de E. David GBADAMASSI
(Master 2 Droit Privé Fondamental, 2022-2023).

---

## WORKFLOW GÉNÉRAL

```
1. Collecter les données brutes de l'utilisateur
2. Identifier le TYPE de chaque source
3. Détecter les informations manquantes
4. Compléter via recherche web (Google Scholar, bases académiques)
5. Formater selon les normes ED-SJPA
6. Produire le rapport Word
```

---

## ÉTAPE 1 — COLLECTE ET IDENTIFICATION DES TYPES

### Types de sources reconnus

| Code | Type de source |
|------|---------------|
| `OUVRAGE` | Ouvrage général ou spécialisé (1 ou 2 auteurs) |
| `OUVRAGE_MULTI` | Ouvrage avec 3+ auteurs |
| `OUVRAGE_DIR` | Ouvrage collectif sous direction |
| `FASCICULE` | Fascicule de cours |
| `ARTICLE` | Article de doctrine (revue) |
| `CONTRIBUTION` | Contribution dans mélange ou ouvrage collectif |
| `MEMOIRE` | Mémoire de Master |
| `THESE` | Thèse de Doctorat |
| `MELANGE` | Ouvrage de mélanges |
| `RAPPORT` | Rapport, atelier, acte de colloque |
| `ETUDE` | Étude, observation, recueil, discours, émission, communiqué |
| `AUDIOVISUEL` | Support audiovisuel (émission radio, podcast, etc.) |
| `LOI` | Ressource législative (loi, décret, arrêté, etc.) |
| `JURISPRUDENCE` | Décision de justice |
| `LEXIQUE` | Lexique, vocabulaire ou dictionnaire juridique |
| `WEB` | Ressource web / webographie |

---

## ÉTAPE 2 — INFORMATIONS REQUISES PAR TYPE

Consulter le fichier `references/champs-requis.md` pour la liste détaillée des champs
obligatoires et facultatifs par type de source.

---

## ÉTAPE 3 — DÉTECTION ET COMPLÉTION DES INFORMATIONS MANQUANTES

### Stratégie de recherche web

Pour chaque information manquante **critique** (marquée ✦ dans champs-requis.md) :

1. **Chercher d'abord** sur Google Scholar : `"[titre exact]" [auteur]`
2. **Puis** sur les bases : WorldCat, Sudoc, OpenLibrary, base BnF
3. **Pour les articles** : chercher la revue + numéro + pages dans CAIRN, Persée, JSTOR
4. **Pour la jurisprudence** : Légifrance, bases OHADA, bases nationales béninoises
5. **Pour les lois béninoises** : Journal Officiel de la République du Bénin (JORB)

### Politique de tolérance aux données manquantes

- Si une information est introuvable après recherche → **omettre silencieusement** (règle 4 du document de référence)
- Ne JAMAIS inventer ou approximer des données bibliographiques
- Signaler les informations non trouvées dans le rapport final (section "Avertissements")

---

## ÉTAPE 4 — RÈGLES DE FORMATAGE ED-SJPA

### Règles universelles (s'appliquent à tous les types)

**Noms d'auteurs :**
- Format standard : `NOM (Prénom)` → ex. `CORNU (Gérard)` ou `CORNU (G.)`
- Choisir UN format et le respecter dans tout le document
- **Recommandé bibliographie** : `NOM (Prénom complet)` ou `NOM (Initiale.)`
- **Recommandé note de bas de page** : `NOM (Initiale.)` ou `NOM (Prénom complet)`

**Titres :**
- Ouvrages, fascicules, mélanges, mémoires, thèses, lexiques, affaires en cause → ***italique***
- Articles, contributions, rapports, colloques, webographie → entre `« guillemets »`
- Discours, émissions, entretiens, communiqués → `« guillemets »` ou `''griffes''`

**Différence note de bas de page vs bibliographie :**
- Note de bas de page → indique le **numéro de la page consultée** : `p. XX`
- Note bibliographique → indique le **nombre total de pages** : `XX pages` (ou fourchette `pp. XX-YY`)

**Ouvrage modifié (loi) :**
- Note de bas de page : loi modificatrice d'abord, puis loi modifiée
- Note bibliographique : loi modifiée d'abord, puis loi modificatrice

### Formules par type — voir `references/formules-citation.md`

---

## ÉTAPE 5 — GÉNÉRATION DU RAPPORT WORD

### Installer les dépendances

```bash
npm install -g docx
```

### Structure du rapport Word

Le rapport comporte obligatoirement :

1. **Page de titre** : "Rapport de Références Bibliographiques — Normes ED-SJPA/UAC"
2. **Section I — Notes de bas de page** (numérotées, style footnote)
3. **Section II — Références Bibliographiques** (classées par type de source)
4. **Section III — Avertissements** (informations non trouvées, sources douteuses)

### Script de génération

Utiliser le script JavaScript fourni dans `references/generate-rapport.js`.
Lancer avec : `node references/generate-rapport.js`

Le script prend en entrée un objet JSON de sources et produit le fichier `.docx`.

---

## ÉTAPE 6 — VALIDATION FINALE

Avant de livrer le rapport, vérifier :

- [ ] Chaque note de bas de page se termine par `p. XX` (sauf jurisprudence et loi)
- [ ] Chaque référence bibliographique se termine par `XX pages` ou fourchette
- [ ] Les titres d'ouvrages sont en italique
- [ ] Les titres d'articles sont entre guillemets
- [ ] Le format des noms d'auteurs est cohérent dans tout le document
- [ ] Les sources avec informations manquantes sont signalées dans les avertissements

---

## EXEMPLE D'UTILISATION

**Entrée utilisateur :**
```
Auteur: Bénabent A.
Titre: Droit des obligations
Édition: 19e
Éditeur: LGDJ
Année: 2018
Page consultée: 76
```

**Sortie attendue :**

*Note de bas de page :*
> BENABENT (A.), *Droit des obligations*, 19e éd., Paris, LGDJ, 2018, p. 76.

*Référence bibliographique :*
> BENABENT (A.), *Droit des obligations*, 19e éd., Paris, LGDJ, 2018, 768 pages.

---

## FICHIERS DE RÉFÉRENCE

- `references/champs-requis.md` — Champs obligatoires/facultatifs par type
- `references/formules-citation.md` — Formules exactes de citation par type
- `references/generate-rapport.js` — Script Node.js de génération du fichier Word
