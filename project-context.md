---
project_name: 'africa-freelancer'
user_name: 'Aris'
date: '2026-06-16'
sections_completed:
  - technology_stack
  - language_rules
  - framework_rules
  - testing_rules
  - quality_rules
  - workflow_rules
  - anti_patterns
status: 'complete'
rule_count: 35
optimized_for_llm: true
existing_patterns_found: 10
---

# Project Context for AI Agents

_Ce fichier contient les règles et patterns critiques que les agents IA doivent suivre lors de l'implémentation de code dans ce projet. Concentre-toi sur les détails non évidents que les agents pourraient autrement manquer._

---

## Technology Stack & Versions

| Technologie | Version |
|---|---|
| React | ^19.2.5 |
| React DOM | ^19.2.5 |
| Vite | ^8.0.10 |
| Tailwind CSS | ^4.3.0 |
| React Router | ^7.15.0 |
| TanStack React Query | ^5.100.9 |
| Axios | ^1.16.0 |
| lucide-react | ^1.14.0 |
| react-hot-toast | ^2.6.0 |
| @vitejs/plugin-react | ^6.0.1 |
| ESLint (flat config) | ^10.2.1 |
| eslint-plugin-react-hooks | ^7.1.1 |
| eslint-plugin-react-refresh | ^0.5.2 |

- **Langage :** JavaScript (JSX), pas de TypeScript
- **Backend :** Django REST API (proxy Vite `/api` → `http://localhost:8000`)
- **Styles :** Tailwind CSS v4 + fichiers CSS globaux pour le design system
- **Build :** Vite avec plugins React et Tailwind
- **Format :** ES Modules (`"type": "module"` dans package.json)

## Critical Implementation Rules

### Language-Specific Rules

- **Fichiers :** `.jsx` pour les composants React, `.js` pour la logique pure (API, utils)
- **Imports :** Chemins relatifs, pas d'alias. Icônes depuis `lucide-react` par nom (`{ Search, Bell }`)
- **Exports :** `export default function Nom` pour les pages/composants ; exports nommés pour hooks et API
- **Async :** Toujours `async/await`. TanStack Query pour les données serveur
- **Erreurs :** `try/catch` + `toast.error()` pour l'UX. Pas de `console.log` en prod
- **État local :** `useState` avec validation manuelle via objet `errors` ; pas de lib externe
- **ES Modules :** `"type": "module"` — `import`/`export` uniquement, pas de `require`

### Framework-Specific Rules

- **Hooks :** `useState` pour état local ; `useEffect` minimal ; `useNavigate`/`useLocation`/`useSearchParams` pour routing
- **TanStack Query :** Query keys en kebab-case `['projets', ...]` ; `staleTime: 30000` défaut ; `queryClient.invalidateQueries` après mutation
- **React Router :** Routes dans `App.jsx` ; `PrivateRoute` pour routes protégées ; Navbar incluse par route
- **Auth :** `AuthContext.jsx` avec hook `useAuth()` ; JWT dans localStorage (access + refresh)
- **API Client :** `src/api/client.js` ; interceptor Axios avec refresh token automatique (401 → refresh → retry)
- **Composants :** Pages dans `src/pages/` ; composants partagés dans `src/components/`

### Code Quality & Style Rules

- **ESLint :** Flat config — `js.configs.recommended` + `react-hooks` + `react-refresh` ; cible `**/*.{js,jsx}`
- **Organisation :** `pages/` (1 fichier par page), `components/` (partagés), `context/` (1 par contexte), `api/` (client centralisé)
- **Nommage :** Composants → PascalCase `.jsx` ; variables/fonctions → camelCase ; constantes → UPPER_SNAKE_CASE
- **Styles :** Design system CSS dans `index.css` avec variables `--green`, `--orange`, `--sand`, etc. ; Tailwind `@import "tailwindcss"` ; fichiers `.css` séparés par page si besoin
- **Pas de CSS Modules ni CSS-in-JS** — utilitaires Tailwind + classes CSS globales
- **Design System :** Palette vert africain (`--green-*`), orange chaleureux (`--orange-*`), tons sable (`--sand-*`) ; polices DM Sans (corps), Syne (titres), Plus Jakarta Sans (alternate)

### Testing Rules

- Aucun framework de test configuré actuellement
- Aucune couverture de tests requise pour le moment

### Development Workflow Rules

- **Git :** Branche `master` comme tronc principal ; branches `fix/` et `remotes/origin/` observées
- **API :** Backend Django attendu sur `localhost:8000` ; proxy Vite configuré pour `/api`
- **Build :** `npm run dev` → Vite dev server ; `npm run build` → Vite build ; `npm run lint` → ESLint
- **Icônes :** Utiliser `lucide-react` — vérifier dans la doc Lucide le nom exact de l'icône avant utilisation
- **Notifications :** `react-hot-toast` pour tous les toasts utilisateur ; position top-right ; font DM Sans

### Critical Don't-Miss Rules

- **JWT localStorage :** Sensible XSS — ne jamais exposer les tokens dans le code ou les logs
- **Pas de TypeScript :** Ne PAS ajouter de fichiers `.ts`/`.tsx` sans demande explicite
- **Validation :** Utiliser validation manuelle avec `useState` + objet `errors` ; ne PAS installer `react-hook-form`/`zod`
- **API proxy :** Utiliser `/api/...` relatif (proxy Vite) — ne PAS hardcoder l'URL backend dans les composants
- **CSS-in-JS interdit :** Pas de styled-components/emotion. Utiliser Tailwind + variables CSS `var(--green)` etc.
- **react-refresh :** Les composants doivent avoir un export default function unique pour HMR
- **TanStack Query v5 :** Syntaxe `queryKey: [...], queryFn: ...` (pas `_key` ou propriété `query`)
- **Commission :** 10% de commission plateforme toujours déduite du budget freelance (calcul dans CreateProject)
- **Breakpoints :** Utiliser les media queries du design system existant (768px, 480px) ; ne pas en ajouter de nouvelles sans vérifier
- **Fonts :** DM Sans (corps), Syne (titres) ; chargées via Google Fonts dans `index.html`

---

## Usage Guidelines

**Pour les agents IA :**

- Lire ce fichier avant d'implémenter du code
- Suivre TOUTES les règles exactement comme documenté
- En cas de doute, préférer l'option la plus restrictive
- Mettre à jour ce fichier si de nouveaux patterns émergent

**Pour les humains :**

- Garder ce fichier concis et centré sur les besoins des agents
- Mettre à jour quand la stack technique change
- Réviser trimestriellement pour supprimer les règles obsolètes

Dernière mise à jour : 2026-06-16
