# FastDeploy — Plugin Claude Code

Assistant conversationnel de déploiement souverain pour Claude Code.

Tape `/fastdeploy` + ton idée d'application → Claude te pose 10 questions
guidées (langue, secteur, design, fonctionnalités, hébergement, paiement,
authentification, sécurité) puis déploie une application de base en
production sur infrastructure française (OVH ou Scaleway) avec sécurité
niveau CAC 40.

> 🇫🇷 Souveraineté française · 🛡️ Conforme SecNumCloud / HDS / NIS 2 · ⚡ Production en 5 minutes

---

## Installation

### Pré-requis

- [Claude Code](https://claude.com/claude-code) installé
- Une clé de licence FastDeploy → [obtenir une licence](https://deploysovereign.fr/pricing)

### 1. Installer le plugin

Dans Claude Code, tape :

```
/plugin marketplace add deploysovereign/fastdeploy
/plugin install fastdeploy
```

### 2. Configurer la clé de licence

Après souscription chez [deploysovereign.fr](https://deploysovereign.fr/pricing),
tu reçois ta clé `ds_live_…` par email. Définis-la comme variable
d'environnement :

**macOS / Linux** :
```bash
export FASTDEPLOY_LICENSE_KEY="ds_live_xxxxxxxxxxxxxxxx"
```

Pour la rendre permanente, ajoute la ligne à `~/.bashrc`, `~/.zshrc` ou
`~/.profile`.

**Windows (PowerShell)** :
```powershell
[Environment]::SetEnvironmentVariable("FASTDEPLOY_LICENSE_KEY", "ds_live_xxxxxxxxxxxxxxxx", "User")
```

Redémarre Claude Code pour que la variable soit prise en compte.

### 3. Utiliser

Dans n'importe quelle conversation Claude Code :

```
/fastdeploy app de réservation pour un restaurant
```

Claude lance le wizard conversationnel et te guide jusqu'au déploiement.

---

## Exemple de session

```
Toi : /fastdeploy app de réservation pour un restaurant

Claude : 🍽️ Pour te proposer des maquettes adaptées, dis-moi :
         1. Ton secteur — restauration ?
         2. Ce que fait l'app en une phrase
         3. Tes couleurs de marque

Toi : Restauration, prendre des réservations en ligne, rouge bordeaux et beige.

Claude : Parfait. Voici 3 directions graphiques pour ton app de réservation
         (toutes accessibles AA, mobile-first, avec accès aux statistiques) :

         Direction 1 — Épurée / minimaliste
         ┌────────────────────────────────────┐
         │  CHEZ MARIO                        │
         │  Réservation en ligne              │
         │  [Choisir une date] [Voir le menu] │
         └────────────────────────────────────┘
         …

         Quelle direction préfères-tu — 1, 2 ou 3 ?
```

À la fin, l'application est en ligne sur ton sous-domaine
`https://<ton-nom>.deploysovereign.fr` et tu peux y ajouter des
fonctionnalités par simples prompts.

---

## Ce que le plugin sait faire

| Capacité | Détail |
|---|---|
| **Wizard guidé en 3 langues** | Français / English / Español |
| **Matrice plateforme** | OVH (HDS, SecNumCloud), Scaleway, Vercel, Azure, AWS — avec recommandation contextuelle |
| **Patterns d'auth** | Email/MDP, MFA TOTP, SSO SAML/OIDC, Passkeys/FIDO2 |
| **Niveaux sécurité** | Standard (RGPD) · Entreprise (CAC 40) · Souverain (SecNumCloud/OIV) |
| **Paiements** | Stripe, Lyra/PayZen 🇫🇷, prestataire existant, contrat VAD bancaire |
| **Templates app** | Web statique, API, Full-stack (Next.js + PostgreSQL) |
| **Référentiels** | RGPD, ANSSI, NIS 2, ISO 27001, HDS, SecNumCloud, DORA, PCI-DSS |

---

## Plans

| Plan | Wizard + conseils | Déploiement infra |
|---|---|---|
| **Starter** | ✅ | ✅ (1 portail) |
| **Pro** | ✅ | ✅ (3 portails + app clients) |
| **Business** | ✅ | ✅ (illimité + équipe) |
| **Souverain** | ✅ | ✅ (CAC 40 / public / OIV) |

Détails et tarifs : [deploysovereign.fr/pricing](https://deploysovereign.fr/pricing)

---

## Structure du plugin

```
fastdeploy/
├── .claude-plugin/
│   ├── plugin.json       # Manifeste plugin
│   └── mcp.json          # Connexion au serveur MCP (auth par licence)
├── commands/
│   └── fastdeploy.md     # Le wizard conversationnel
└── skills/
    └── fastdeploy/
        └── SKILL.md      # Base de connaissances (matrice plateforme, etc.)
```

---

## Confidentialité & sécurité

- Le serveur MCP (`mcp.deploysovereign.fr`) reçoit uniquement la clé de licence
  pour authentification. **Aucun code source ni donnée d'utilisateur n'est
  transmis** au serveur DeploySovereign.
- Les déploiements se font directement depuis ton compte cloud
  (OVH/Scaleway). DeploySovereign n'a jamais accès à tes données.
- Sessions MCP via SSE (Server-Sent Events) chiffrées TLS 1.3.

---

## Support

- 📧 [support@deploysovereign.fr](mailto:support@deploysovereign.fr)
- 🌐 [deploysovereign.fr](https://deploysovereign.fr)
- 📚 [Documentation](https://deploysovereign.fr/docs/install)

---

## Licence

Plugin sous licence propriétaire DeploySovereign. Utilisation conditionnée à une
souscription active. Voir [LICENSE](./LICENSE).
