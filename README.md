# 📚 Citation Juridique Béninoise — Skill ED-SJPA/UAC

<div align="center">

![Version](https://img.shields.io/badge/version-1.0.0-blue?style=flat-square)
![Licence](https://img.shields.io/badge/licence-MIT-green?style=flat-square)
![Node.js](https://img.shields.io/badge/Node.js-%3E%3D16-brightgreen?style=flat-square&logo=node.js)
![Normes](https://img.shields.io/badge/normes-ED--SJPA%2FUAC-orange?style=flat-square)
![Statut](https://img.shields.io/badge/statut-stable-success?style=flat-square)

**Génération automatique de notes de bas de page et de références bibliographiques
selon les normes académiques de l'ED-SJPA — Université d'Abomey-Calavi, Bénin.**

[Fonctionnalités](#-fonctionnalités) · [Installation](#-installation) · [Utilisation](#-utilisation) · [Structure](#-structure-du-projet) · [Exemples](#-exemples) · [Contribuer](#-contribuer)

</div>

---

## 🎯 À propos

Ce projet est un **skill Claude AI** conçu pour automatiser la mise en forme des citations
juridiques selon les normes de l'École Doctorale des Sciences Juridiques, Politiques et
Administratives (**ED-SJPA**) de l'**Université d'Abomey-Calavi (UAC)**, Bénin.

Il repose sur le fascicule de référence :

> E. David GBADAMASSI, *Notes de bas de pages et notes bibliographiques : Rédaction pratique*,
> UAC/ED-SJPA, Master 2 Droit Privé Fondamental, Année 2022-2023.

### Pourquoi ce projet ?

La rédaction des notes de bas de page et des références bibliographiques est l'une des
étapes les plus techniques — et les plus chronophages — de la rédaction d'un mémoire ou
d'une thèse en droit. Une erreur de format peut compromettre la rigueur scientifique d'un
travail par ailleurs excellent. Ce skill élimine ce risque en automatisant entièrement la
mise en forme, tout en complétant les informations manquantes par recherche web.

---

## ✨ Fonctionnalités

- **16 types de sources** pris en charge : ouvrages, articles, thèses, mémoires, lois,
  jurisprudences, webographie, rapports, mélanges, fascicules de cours, et plus encore
- **Détection automatique** des informations manquantes dans les références fournies
- **Complétion par recherche web** (Google Scholar, WorldCat, Sudoc, Légifrance, CAIRN, JORB…)
- **Application stricte** des règles ED-SJPA : italiques, guillemets, ordre des éléments,
  règles spéciales (`et alii.`, `inédit`, loi modifiée/modificatrice…)
- **Génération d'un rapport Word** (.docx) structuré en 3 sections :
  - Section I — Notes de bas de page numérotées
  - Section II — Références bibliographiques classées par catégorie
  - Section III — Avertissements (informations introuvables à vérifier manuellement)
- **Robustesse** aux données incomplètes : les champs introuvables sont omis proprement
  conformément à la règle 4 du fascicule de référence
- **Configuration YAML** complète pour intégration dans des workflows d'édition académique

---

## 🗂 Structure du projet

```
citation-juridique-beninoise/
│
├── SKILL.md                          # Instructions principales (workflow 6 étapes)
├── citation-juridique-beninoise.yaml # Configuration complète du skill
├── README.md                         # Ce fichier
├── LICENSE                           # Licence MIT
│
└── references/
    ├── champs-requis.md              # Champs obligatoires/facultatifs par type de source
    ├── formules-citation.md          # 17 formules exactes de citation avec exemples
    └── generate-rapport.js           # Script Node.js de génération du rapport Word
```

---

## ⚙️ Installation

### Prérequis

- [Node.js](https://nodejs.org/) version 16 ou supérieure
- [Claude AI](https://claude.ai) avec accès aux skills (Claude Code ou claude.ai)

### 1. Cloner le dépôt

```bash
git clone https://github.com/saddamsahagui/citation-juridique-beninoise.git
cd citation-juridique-beninoise
```

### 2. Installer les dépendances Node.js

```bash
npm install -g docx
```

### 3. Installer le skill dans Claude

**Option A — Fichier `.skill` (recommandé)**

Télécharger `citation-juridique-beninoise.skill` et l'importer dans votre environnement Claude.

**Option B — Manuel**

Copier le dossier `citation-juridique-beninoise/` dans votre répertoire de skills :

```bash
cp -r citation-juridique-beninoise/ /mnt/skills/user/
```

---

## 🚀 Utilisation

### Utilisation via Claude AI

Une fois le skill installé, il se déclenche automatiquement dès que vous fournissez des
données bibliographiques à Claude. Exemples de phrases déclencheuses :

```
"Génère mes notes de bas de page à partir de ces références..."
"Formate ma bibliographie selon les normes ED-SJPA"
"Crée un rapport de citations juridiques pour mon mémoire"
"Complète et formate ces références bibliographiques incomplètes"
```

### Utilisation directe du script Node.js

Vous pouvez également utiliser le script de génération Word indépendamment de Claude :

#### 1. Préparer votre fichier de sources (JSON)

```json
{
  "titre_rapport": "Mémoire de Master 2 — Droit des Affaires",
  "auteur_travail": "SAHAGUI K. Saddam",
  "date_generation": "18/03/2026",
  "sources": [
    {
      "id": "src001",
      "type": "OUVRAGE",
      "categorie": "Ouvrages généraux et spécialisés",
      "note_bas_page": "BENABENT (A.), *Droit des obligations*, 19e éd., Paris, LGDJ, 2018, p. 76.",
      "note_biblio": "BENABENT (A.), *Droit des obligations*, 19e éd., Paris, LGDJ, 2018, 768 pages.",
      "avertissements": []
    },
    {
      "id": "src002",
      "type": "LOI",
      "categorie": "Ressources législatives",
      "note_bas_page": "Article 326 de la loi n°2017-20 du 20 avril 2018 portant code du numérique en République du Bénin.",
      "note_biblio": "Loi n°2017-20 du 20 avril 2018 portant code du numérique en République du Bénin, 620 pages.",
      "avertissements": []
    }
  ]
}
```

#### 2. Générer le rapport Word

```bash
node references/generate-rapport.js sources.json mon-rapport.docx
```

#### 3. Sans fichier source (données d'exemple)

```bash
node references/generate-rapport.js
# → génère rapport-citations-edspa.docx avec 3 sources d'exemple
```

---

## 📋 Exemples

### Ouvrage (1 auteur)

| | Résultat |
|---|---|
| **Note de bas de page** | BENABENT (A.), *Droit des obligations*, 19e éd., Paris, LGDJ, 2018, p. 76. |
| **Note bibliographique** | BENABENT (A.), *Droit des obligations*, 19e éd., Paris, LGDJ, 2018, 768 pages. |

### Article de doctrine

| | Résultat |
|---|---|
| **Note de bas de page** | GAUTRAIS (V.), « Les Principes d'UNIDROIT face au contrat électronique », *R.J.T.*, n°36, 2002, p. 485. |
| **Note bibliographique** | GAUTRAIS (V.), « Les Principes d'UNIDROIT face au contrat électronique », *R.J.T.*, n°36, 2002, pp. 481-518. |

### Ressource législative (avec loi modifiée)

| | Résultat |
|---|---|
| **Note de bas de page** | Article 14 de la loi n°2016-15 du 28 juillet 2016 modifiant et complétant la loi n°2001-37 du 27 août 2002 portant organisation judiciaire en République du Bénin. |
| **Note bibliographique** | Loi n°2001-37 du 27 août 2002 portant organisation judiciaire en République du Bénin modifié par la loi n°2016-15 du 28 juillet 2016, 19 pages. |

### Webographie

| | Résultat |
|---|---|
| **Note de bas de page** | LEGRAND (V.), « La loi applicable au contrat de commerce électronique », *actujuridique.fr*, publié le 8 septembre 2017 à 0h 00mn, consulté le 16 décembre 2022 à 17h 53mn. |
| **Note bibliographique** | LEGRAND (V.), « La loi applicable au contrat de commerce électronique », *actujuridique.fr*, publié le 8 septembre 2017 à 0h 00mn. |

---

## 📐 Types de sources supportés

| Code | Type de source |
|------|---------------|
| `OUVRAGE` | Ouvrage général ou spécialisé (1 ou 2 auteurs) |
| `OUVRAGE_MULTI` | Ouvrage avec 3+ auteurs (`et alii.`) |
| `OUVRAGE_DIR` | Ouvrage collectif sous direction |
| `FASCICULE` | Fascicule de cours universitaire |
| `ARTICLE` | Article de doctrine dans une revue |
| `CONTRIBUTION` | Contribution dans un mélange ou ouvrage collectif |
| `MEMOIRE` | Mémoire de Master |
| `THESE` | Thèse de Doctorat |
| `MELANGE` | Ouvrage de mélanges (en l'honneur de…) |
| `RAPPORT` | Rapport, atelier ou acte de colloque |
| `ETUDE` | Étude, observation, recueil, discours, émission, communiqué |
| `AUDIOVISUEL` | Support audiovisuel (émission, podcast…) |
| `LOI` | Ressource législative (loi, décret, arrêté) |
| `JURISPRUDENCE` | Décision de justice |
| `LEXIQUE` | Lexique, vocabulaire ou dictionnaire juridique |
| `WEB` | Ressource web / webographie |

---

## 🔍 Sources de vérification et complétion

Lorsque des informations sont manquantes, le skill effectue des recherches sur :

- [Google Scholar](https://scholar.google.com)
- [WorldCat](https://www.worldcat.org) / [Sudoc](https://www.sudoc.fr) / [BnF](https://catalogue.bnf.fr)
- [CAIRN](https://www.cairn.info) / [Persée](https://www.persee.fr) / [JSTOR](https://www.jstor.org)
- [Légifrance](https://www.legifrance.gouv.fr)
- Journal Officiel de la République du Bénin (JORB)
- Bases de données OHADA

---

## 🤝 Contribuer

Les contributions sont les bienvenues ! Pour contribuer :

1. Forker le dépôt
2. Créer une branche pour votre fonctionnalité (`git checkout -b feature/nouveau-type-source`)
3. Commiter vos modifications (`git commit -m 'Ajout : support du type ENCYCLOPEDIE'`)
4. Pousser la branche (`git push origin feature/nouveau-type-source`)
5. Ouvrir une Pull Request

### Idées d'amélioration

- [ ] Support des normes APA et Chicago en plus des normes ED-SJPA
- [ ] Interface web pour saisie interactive des sources
- [ ] Export en format BibTeX / Zotero
- [ ] Support des sources en langues anglaise et portugaise
- [ ] Intégration avec Zotero et Mendeley via API

---

## 🐛 Signaler un problème

Si vous rencontrez un bug ou souhaitez suggérer une amélioration, ouvrez une
[issue GitHub](https://github.com/saddamsahagui/citation-juridique-beninoise/issues)
en précisant :

- La version du skill utilisée
- Le type de source concerné
- Les données d'entrée fournies
- Le comportement observé vs. attendu

---

## 📖 Référence normative

Ce skill implémente fidèlement les règles définies dans :

> **GBADAMASSI (E. D.)**, *Notes de bas de pages et notes bibliographiques : Rédaction pratique — L'essentiel à retenir*,
> UAC/ED-SJPA, Master 2 Droit Privé Fondamental, Année 2022-2023.
>
> Disponible sur : [clicdroitfacie.wordpress.com](https://clicdroitfacie.wordpress.com)

---

## 👥 Auteurs

<table>
  <tr>
    <td align="center">
      <b>SAHAGUI K. Saddam</b><br/>
      <sub>Concepteur & Développeur principal</sub><br/>
      <a href="https://github.com/saddamsahagui">@saddamsahagui</a>
    </td>
    <td align="center">
      <b>Claude AI</b><br/>
      <sub>Intelligence Artificielle — Anthropic</sub><br/>
      <a href="https://claude.ai">claude.ai</a>
    </td>
  </tr>
</table>

---

## 📄 Licence

Ce projet est distribué sous licence **MIT**. Voir le fichier [LICENSE](LICENSE) pour les
détails complets.

---

<div align="center">
  <sub>Fait avec ❤️ pour la communauté juridique béninoise et francophone</sub><br/>
  <sub>© 2026 SAHAGUI K. Saddam & Claude AI — Anthropic</sub>
</div>
