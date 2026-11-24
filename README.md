# ✦ NOVA — Beyond Slides

**NOVA** est un studio de présentation nouvelle génération, développé en **C# / Windows Forms (.NET 10)**.
L'intelligence artificielle ne se contente pas de créer les diapositives : elle comprend l'idée,
construit l'histoire et accompagne la personne qui présente, avant, pendant et après la présentation.

> Projet de recherche — cours de Programmation C#, Bachelor 2, KEYCE Informatique & IA (2026-2027).

## Fonctionnalités

### Les bases d'un PowerPoint

| Fonction | Description |
|---|---|
| 🔐 Compte et verrouillage | Inscription, mot de passe et code PIN hachés (PBKDF2 + sel), écran verrouillé à l'ouverture |
| 🏠 Pages | Accueil, Mes projets (rechercher, renommer, dupliquer, supprimer), Bibliothèque de modèles, Paramètres |
| 🎨 Studio | Ruban Format (police, taille, gras, italique, alignement, couleur), 8 poignées de redimensionnement, premier plan / arrière-plan |
| ⌨ Raccourcis | Ctrl+Z / Ctrl+Y, Ctrl+C / X / V, Ctrl+D (dupliquer), Ctrl+M (nouvelle diapo), Suppr, flèches, F5 (présenter) |
| 🖼 Insertion | Textes, formes (rectangle, arrondi, cercle, flèche, trait), images depuis le PC, objets 3D animés dessinés en code |
| 🖌 Thèmes | 7 thèmes (NOVA Noir, Bordeaux Royal, Argent Studio, Minuit, Émeraude, Papier, Néon) appliqués à toute la présentation |
| ✨ Transitions | Fondu, Glisser, Pousser, Zoom, Volet, Cinéma — aperçu animé, vitesse réglable, « Appliquer à toutes » |
| 🎬 Timeline | Animations par élément : apparition, glissement, zoom, montée, réglables à la souris |
| 🖥 Mode présentation | Plein écran, 2 écrans, pointeur laser (L), écran noir / blanc (B / W), numéro + Entrée pour aller à une diapo |
| 📥 Import | Ouvre un fichier PowerPoint (.pptx) existant |
| 📤 Export | PowerPoint (.pptx modifiable, avec transitions), PDF, images PNG, **vidéo AVI** (transitions + animations) |
| 📖 NOVA Academy | 8 leçons interactives avec quiz (dont « Concevoir un modèle ») |

### Les fonctionnalités NOVA (IA)

| Fonction | Description |
|---|---|
| 🤖 NOVA Assistant | Conversation avec l'IA Claude, qui connaît la présentation ouverte et son public |
| ✦ Mind-to-Deck | Une idée → une présentation complète (plan, textes, notes de l'orateur) |
| 🎯 Audience Intelligence | Réécrit toute la présentation pour un autre public (étudiants, jury, investisseurs…) |
| 🎤 Live AI Director | Écran privé : chrono, notes, diapo suivante, aide de l'IA en direct |
| 🎙 AI Rehearsal | Répétition au micro : chrono, débit (mots/min), tics de langage (« euh », « du coup »), puis coach IA (note /20, conseils, 5 questions du jury) |
| 💬 Écoute du public | Sous-titres en direct ; les questions du public sont repérées et l'IA souffle une réponse |
| ✦ Living Presentation | Une question du public → l'IA crée une diapo temporaire avec un schéma (étapes, cycle, comparaison, hiérarchie) |
| ∞ Infinite Story | L'IA imagine la suite de la présentation, dans le même thème |
| 🌍 Global Voice | Traduction de toute la présentation, et sous-titres traduits en direct (touche T) |

## Architecture (en couches)

```
NOVA.Winforms   → Interface : formulaires, contrôles dessinés (canevas, timeline, 3D), micro, export
     │
NOVA.Core       → Modèles      : Utilisateur, Presentation, Diapositive, ElementDiapo (abstraite), ThemeDiapo …
                → Services     : CompteService, PresentationService, AssistantIA, AcademyService,
                                 SecuriteService, ImportPptxService, Historique, AnalyseOrale
                → Persistance  : DepotJson<T> (System.Text.Json)
                → Exceptions   : CompteException, PresentationException, IAException
NOVA.Tests      → Tests xUnit de la couche Core
```

L'interface ne lit jamais le JSON directement : elle passe toujours par les services.

## Notions du cours mises en œuvre

- **POO** : classe abstraite `ElementDiapo` et classes dérivées (`ElementTexte`, `ElementForme`, `ElementIllustration`, `ElementImage`), **polymorphisme** (dessin, sérialisation JSON)
- **Exceptions personnalisées** pour les règles métier
- **Collections et LINQ** (`OfType`, `Select`, `Max`, `OrderByDescending`…)
- **Fichiers et JSON** avec `System.Text.Json`, lecture et écriture de fichiers ZIP / XML (PowerPoint) et binaires (vidéo AVI)
- **async / await** et `Task.Run` : l'interface reste réactive pendant l'IA et l'export vidéo
- **Événements** et contrôles personnalisés (`OnPaint`, souris, clavier)
- **Validation** des saisies avec `ErrorProvider`
- **Sécurité** : hachage PBKDF2, chiffrement DPAPI de la clé API
- **Tests unitaires xUnit** sur la logique métier

## Lancer le projet

1. Ouvrir `NOVA.slnx` dans **Visual Studio 2026** (.NET 10).
2. Définir **NOVA.Winforms** comme projet de démarrage, puis **Générer > Régénérer la solution** et **F5**.
3. Pour l'IA : créer une clé sur [console.anthropic.com](https://console.anthropic.com), puis la coller dans **Paramètres**.
   La clé est **chiffrée avec DPAPI** sur le PC : elle n'est jamais dans le code ni sur GitHub.
4. Pour le micro : installer la **reconnaissance vocale française** de Windows
   (Paramètres > Heure et langue > Langue et région > Français > Options de langue).

## Raccourcis en présentation

| Touche | Action |
|---|---|
| → / Espace | Diapo suivante |
| ← | Diapo précédente |
| Numéro + Entrée | Aller à une diapo |
| L | Pointeur laser |
| B / W | Écran noir / blanc |
| M | Micro + sous-titres |
| T | Sous-titres traduits |
| E | Diapo explicative (Living Presentation) |
| D | Live AI Director |
| F1 | Aide |
| Échap | Quitter |

## Tests

**Test → Exécuter tous les tests** dans Visual Studio. Les tests utilisent des fichiers temporaires :
ils ne touchent jamais aux vraies données de l'utilisateur.

## Données

Tout est enregistré dans `%AppData%\NOVA` : `compte.json`, `presentations.json`, `academy.json`, `cle-ia.dat` (chiffré).
