# FCM Router — Analyse UX & Recommandations

> **Date**: 2026-04-25
> **Scope**: Tout le code ajouté depuis la dernière release (phases 0–6)
> **Focus**: UX du TUI, workflow utilisateur, ergonomie du router

---

## Vue d'ensemble

Le backend du router est **solide** — circuit breakers, failover, token tracking, SSE, probes, tout est là et bien testé. Le problème est côté **product/UX** : l'interface TUI empile trop de couches, les shortcuts sont cryptiques, et le workflow d'un nouvel utilisateur qui découvre le router est confus.

---

## 🔴 Problèmes Critiques

### 1. Onboarding confus et agressif

**Fichiers**: `overlays.js:1621–1668`, `app.js:1146–1150`, `key-handler.js:2008–2075`

- L'onboarding popup apparaît dès le premier lancement — l'utilisateur n'a **même pas encore vu la table** qu'on lui parle de "Smart Router". Il n'a aucun contexte pour décider.
- Le banner d'upgrade (10s TTL) est affiché aux existing users mais s'auto-dismiss trop vite. L'utilisateur qui lit lentement le rate.
- Deux mécanismes de découverte (overlay onboarding + banner inline) pour la même feature = confusion. L'utilisateur ne sait pas si c'est la même chose.
- **Aucune explication claire** de ce que le router fait concrètement. "Routes your requests to the fastest healthy model" est vague. L'utilisateur ne comprend pas le bénéfice concret.

**Recommandation**:
- Supprimer l'onboarding popup intrusif. Laisser l'utilisateur explorer la table d'abord.
- Remplacer par un hint discret dans le footer : `💡 New: Smart Router — Shift+R to learn more`
- Dans le dashboard (Shift+R), si le daemon n'est pas démarré, afficher un vrai guide "Getting Started" avec les étapes 1-2-3, pas juste un Yes/No popup.
- Expliquer le bénéfice en une phrase concrète : "Point your coding tool at ONE endpoint. FCM auto-switches between your fastest providers when one goes down."

---

### 2. Surcharge cognitive de raccourcis clavier

**Fichier**: `key-handler.js` (2700+ lignes), README TUI Keys section

Le router ajoute **6 nouveaux Shift+ raccourcis** (Shift+R, Shift+S, Shift+T, Shift+A, Shift+U) au-dessus des 25+ raccourcis existants. Le problème :

- `Shift+R` = Router Dashboard, mais `R` = Sort by Rank. Un appui trop long sur R et l'utilisateur atterrit dans un dashboard qu'il ne connaît pas.
- `Shift+S` = Set Manager, mais `S` = Sort by SWE. Même piège.
- `Shift+T` = Token Usage, mais `T` = Cycle Tier. Idem.
- `Shift+A` = Position Picker. Jamais expliqué dans la table principale. L'utilisateur ne sait pas que ça existe.
- Aucun de ces raccourcis n'est visible dans le footer de la table principale. L'utilisateur doit lire le README pour les découvrir.

**Recommandation**:
- Ajouter une ligne de footer dédiée au router quand le daemon est running : `Shift+R Dashboard  •  Shift+S Sets  •  Shift+T Tokens`
- Envisager de regrouper les features router sous **un seul point d'entrée** : Shift+R ouvre le dashboard, et depuis le dashboard on accède aux Sets (S), Tokens (T), etc. Pas besoin de 4 raccourcis différents depuis la table principale.
- Ajouter ces actions dans la command palette (Ctrl+P) — c'est déjà partiellement fait, mais les rendre plus visibles avec des descriptions claires.

---

### 3. Router Dashboard trop dense et peu lisible

**Fichier**: `router-dashboard.js:795–893`

Le dashboard affiche **tout** en une seule vue scrollable :
- Status du daemon (port, PID, SSE, set, uptime, requests, in-flight, probes...)
- Model Health / Circuit Breakers (12 modèles max)
- Token Summary (today + all-time)
- Live Request Log (10 dernières)
- Footer avec tous les raccourcis

Problèmes :
- **Aucune hiérarchie visuelle**. Tout est au même niveau. L'info critique (daemon running? quel set?) est noyée dans les détails.
- Le Model Health table utilise des colonnes fixes (`padEndDisplay`) qui cassent sur des terminaux < 120 cols. Le modèle `moonshotai/kimi-k2.5` fait 23 chars et déborde de la colonne de 30.
- Les états circuit breaker (`● CLOSED`, `◐ HALF`, `○ OPEN`) sont peu intuitifs. L'utilisateur moyen ne sait pas ce qu'est un circuit breaker.
- Le request log montre `request_id` qui est un UUID — complètement inutile pour l'utilisateur.
- Le footer hotkeys liste `R Restart (Phase 7)` et `P Pause probes (disabled)` — des features **non implémentées**. Ça donne l'impression d'un produit inachevé.

**Recommandation**:
- **Reorganiser en sections avec séparateurs visuels** et des titres en gras. Le status du daemon doit être une "status bar" compacte en haut, pas 3 lignes de texte.
- Remplacer le jargon circuit breaker par des labels humains : `✅ Healthy`, `⚠️ Degraded`, `❌ Down`, `🔑 Auth Error`
- Supprimer les request_id du log. Montrer : `Time | Model | Status | Latency` — c'est tout ce que l'utilisateur veut voir.
- **Retirer les hotkeys non implémentés** du footer. Pas de `(Phase 7)` ou `(disabled)` — ça ne devrait jamais être visible par l'utilisateur.
- Ajouter un indicateur visuel clair "daemon ON/OFF" avec couleur — un gros `● RUNNING` vert ou `○ STOPPED` rouge en haut à droite.

---

### 4. Set Manager (Shift+S) workflow fragile

**Fichier**: `key-handler.js` (sections set manager), `router-dashboard.js:605–748`

- Créer un set demande de taper un nom, puis d'ajouter des modèles un par un via une API REST. Pas de sélection visuelle.
- Reorder via `Shift+↑/↓` — mais l'utilisateur ne sait pas que l'ordre compte (priorité de routing). Aucune explication inline.
- Dupliquer/Renommer/Supprimer sont des actions destructives sans confirmation.
- La relation entre les sets du router et les favoris (F key) n'est pas claire. En réalité, `ensureRouterConfigForDaemon()` rebuild le set depuis les favoris, mais l'utilisateur ne le sait pas.

**Recommandation**:
- **Lier explicitement les favoris au router**. L'utilisateur star des modèles avec F, et ces favoris deviennent automatiquement le router set. Pas besoin d'un "Set Manager" séparé pour 90% des users.
- Si on garde le Set Manager, ajouter un mode "pick from table" : l'utilisateur navigue dans la table principale, appuie sur une touche pour ajouter le modèle au set actif.
- Ajouter une confirmation avant suppression de set.
- Montrer clairement `#1 Priority`, `#2 Priority` à côté de chaque modèle dans le set, et expliquer dans un subtitle : "Higher priority = tried first when routing"

---

### 5. Trop d'overlays empilés

**Fichier**: `app.js:1152–1178`

La cascade d'overlays dans le render loop est devenue ingérable :
```
settingsOpen → installEndpointsOpen → toolInstallPromptOpen → installedModelsOpen → 
routerDashboardOpen → tokenUsageOpen → routerOnboardingOpen → incompatibleFallbackOpen → 
commandPaletteOpen → recommendOpen → feedbackOpen → helpVisible → changelogOpen → tableContent
```

C'est **14 états mutuellement exclusifs** gérés par un if/else chain. Problèmes :
- Pas de stack d'overlay. Si l'utilisateur ouvre Settings (P), puis veut voir le Router Dashboard (Shift+R), il doit d'abord fermer Settings.
- Pas de "breadcrumb" — quand on est dans un overlay, on ne sait pas comment on est arrivé là ni comment revenir.
- Le priority order est hardcodé. Si `routerDashboardOpen` et `tokenUsageOpen` sont tous deux true (bug), seul le dashboard s'affiche.

**Recommandation**:
- Implémenter un **overlay stack** : chaque overlay push/pop sur une pile. Esc = pop. Ça permet de naviguer naturellement entre Router Dashboard → Set Manager → retour au Dashboard.
- Afficher un breadcrumb en haut : `Main Table > Router Dashboard > Set Manager`
- Assurer qu'un seul overlay peut être "open" à la fois (invariant) plutôt que de compter sur le priority order.

---

## 🟡 Problèmes Modérés

### 6. Token Usage screen (Shift+T) déconnecté

- Le screen fait un fetch direct vers le daemon. Si le daemon n'est pas running, l'utilisateur voit "Request timed out — is the router daemon running?" — un message technique peu aidant.
- Pas de lien vers le daemon start. L'utilisateur doit savoir faire `free-coding-models --daemon-bg`.
- Le 7-day history chart est en ASCII braille — c'est cool mais peu lisible sur beaucoup de fonts/terminaux.

**Recommandation**:
- Si daemon pas running, afficher : "Router daemon is not running. Start it with `--daemon-bg` or press Enter to start it now." + action button.
- Utiliser des block chars (▁▂▃▄▅▆▇█) pour le chart au lieu de braille — plus universellement lisible.

---

### 7. State explosion dans l'objet `state`

**Fichier**: `app.js:400–557`

L'objet state contient maintenant **150+ propriétés** dont ~40 pour le router seul :
```
routerDashboardOpen, routerDashboardPort, routerDashboardBaseUrl, routerDashboardHealth,
routerDashboardStats, routerDashboardStatus, routerDashboardError, routerDashboardScrollOffset,
routerDashboardEvents, routerDashboardLiveRequests, routerDashboardClearedAt, ...
```

Ce n'est pas un problème UX direct, mais ça rend le code **très difficile à maintenir** et augmente le risque de bugs d'état incohérent.

**Recommandation**:
- Grouper le state du router dans un sous-objet : `state.router = { dashboard: {}, sets: {}, tokens: {}, onboarding: {} }`
- Ça ne change rien au runtime mais améliore la lisibilité et permet de reset tout le state router d'un coup.

---

### 8. Footer de la table principale trop chargé

**Fichier**: `render-table.js`

Quand le router est running, le footer montre déjà beaucoup d'info : mode, tier, filtres, update banner, release date, **et maintenant** le router status. Sur un terminal de 80 cols, ça déborde.

**Recommandation**:
- Condenser le footer router en une seule icône+texte compact : `🔀 fast-coding (3) 1.2k tok` 
- Le détail est dans le dashboard (Shift+R), pas besoin de tout mettre dans le footer.

---

### 9. Daemon start invisible

Quand l'utilisateur active le router via l'onboarding, le daemon démarre en background. Mais :
- Pas d'indication dans le footer principal que le daemon tourne (sauf si on scroll down pour voir le status).
- Si le daemon crash, l'utilisateur ne le sait pas jusqu'à ce qu'il ouvre le dashboard.
- Pas de notification quand le daemon s'arrête (SIGTERM, crash).

**Recommandation**:
- Ajouter un indicateur persistant dans le header de la table : `🔀 Router ●` (vert) ou `🔀 Router ○` (gris).
- Quand le daemon crash, afficher un toast notification de 5s dans la table : "⚠ Router daemon stopped unexpectedly"

---

### 10. syncFavoritesToRouter() silently overwrites sets

**Fichier**: `key-handler.js:378–401`

Quand l'utilisateur lance un modèle (Enter), `syncFavoritesToRouter()` réécrit le set `fast-coding` avec [selected, ...favorites]. Ça écrase silencieusement tout set custom que l'utilisateur aurait pu créer via le Set Manager.

**Recommandation**:
- Ne sync que si le set actif est `fast-coding` (le set par défaut).
- Si l'utilisateur a créé des sets customs, ne pas les toucher.
- Afficher un warning si sync va écraser un set modifié manuellement.

---

## 🟢 Améliorations Quick-Win

### 11. Messages d'erreur techniques exposés à l'utilisateur

- `"Malformed JSON from http://127.0.0.1:19280/stats"` → devrait être `"Router returned unexpected data. Try restarting the daemon."`
- `"Router daemon is not reachable"` → `"Router is not running. Press Shift+R to start it."`
- `"SSE HTTP 503"` → jamais montrer ça à l'utilisateur.

### 12. Pas de "Quick Start" intégré

L'utilisateur qui veut utiliser le router doit lire le README pour connaître le Base URL, Model name, et API key. Ça devrait être un écran copyable directement dans le dashboard :

```
┌─ Quick Setup ─────────────────────────┐
│  Base URL:  http://localhost:19280/v1  │
│  Model:     fcm                        │
│  API Key:   fcm-local                  │
│                                        │
│  Copy these into your coding tool.     │
└────────────────────────────────────────┘
```

### 13. Commande palette incomplète pour le router

Les actions router dans Ctrl+P sont basiques. Il manque :
- "Start router daemon" / "Stop router daemon"
- "Show router connection info" (le Quick Start ci-dessus)
- "Switch router set" (sans aller dans le dashboard)

### 14. Dev mode non documenté

`FCM_DEV=1` utilise des ports et fichiers séparés (`-dev` suffix). C'est bien pensé mais jamais mentionné dans les docs. Les contributeurs ne savent pas que ça existe.

---

## 📋 Priorités de mise en œuvre

| Prio | # | Recommandation | Impact UX | Effort |
|------|---|---------------|-----------|--------|
| 🔴 | 1 | Refaire l'onboarding (hint footer + guide dans dashboard) | Très élevé | Moyen |
| 🔴 | 3 | Nettoyer le dashboard (labels humains, retirer Phase 7, layout) | Élevé | Moyen |
| 🔴 | 2 | Rationaliser les raccourcis (point d'entrée unique Shift+R) | Élevé | Faible |
| 🟡 | 12 | Quick Start copyable dans le dashboard | Élevé | Faible |
| 🟡 | 11 | Humaniser les messages d'erreur | Moyen | Faible |
| 🟡 | 4 | Lier favoris ↔ router set explicitement | Élevé | Moyen |
| 🟡 | 5 | Overlay stack (breadcrumbs, push/pop) | Moyen | Élevé |
| 🟡 | 6 | Token Usage: daemon-start inline + meilleur chart | Moyen | Faible |
| 🟡 | 9 | Indicateur daemon persistant dans header | Moyen | Faible |
| 🟡 | 10 | Ne pas écraser les sets customs lors de sync favorites | Moyen | Faible |
| 🟢 | 8 | Condenser le footer router | Faible | Faible |
| 🟢 | 13 | Actions router dans command palette | Faible | Faible |
| 🟢 | 7 | Grouper state router en sous-objet | Maintenabilité | Moyen |
| 🟢 | 14 | Documenter dev mode | Maintenabilité | Faible |

---

## Résumé

Le backend router est **prêt pour la production**. Le problème est que l'UX a été construite "phase par phase" sans vision globale du parcours utilisateur. Le résultat est une accumulation de screens, overlays, et raccourcis qui n'ont pas de cohérence entre eux.

La refonte prioritaire est de **simplifier le parcours** :
1. L'utilisateur découvre le router via un hint discret → pas un popup
2. Il ouvre le dashboard avec Shift+R → tout est là (status, sets, tokens, setup guide)
3. Il copie les 3 lignes de config dans son outil → c'est fini
4. Les favoris qu'il star dans la table sont automatiquement le router set

Tout le reste (Set Manager, Position Picker, Token Usage) devrait être accessible **depuis le dashboard**, pas comme des raccourcis séparés depuis la table principale.
