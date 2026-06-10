# FastDeploy — Plugin Claude Code

Assistant conversationnel de déploiement souverain. Tape `/fastdeploy`, décris ton
app, réponds à quelques questions, et obtiens une application de base en production
sur infrastructure française avec sécurité niveau CAC 40.

---

## Installation (pour ton client et ses clients)

### Étape 1 — Activer le plugin dans Claude Code

Dans Claude Code, ouvrir les connecteurs / plugins :

```
/plugin marketplace add deploysovereign/fastdeploy
/plugin install fastdeploy
```

Ou via le menu **Connectors** → rechercher "FastDeploy" → **Activer**.

### Étape 2 — Configurer la clé de licence

Après souscription, l'utilisateur reçoit sa clé. L'ajouter comme variable
d'environnement :

```bash
export FASTDEPLOY_LICENSE_KEY="ds_live_xxxxxxxxxxxxxxxx"
```

Ou dans la configuration Claude Code.

### Étape 3 — Utiliser

Dans n'importe quelle conversation :

```
/fastdeploy app de réservation pour un restaurant
```

Claude lance alors le wizard conversationnel.

---

## Exemple de session

```
Utilisateur : /fastdeploy app de réservation pour un restaurant

Claude : Super projet ! 🍽️ Je comprends que tu veux déployer une application
         de réservation pour un restaurant. Je vais te poser 5 questions
         rapides pour configurer ton déploiement souverain.

         1️⃣ D'abord, le type d'application. Une réservation en ligne implique
         généralement du full-stack (interface + base de données). Je pars
         là-dessus, ça te convient ? (oui / ou précise)

Utilisateur : oui

Claude : Full-stack ✓ — interface client + back-office + base de données.

         2️⃣ Où héberger tes données ? Comme tu gères des données clients
         (noms, téléphones, emails de réservation), je recommande la
         souveraineté française :

         a) OVH 🇫🇷 — données 100% en France (recommandé)
         b) Scaleway 🇫🇷 — souveraineté française
         c) Vercel 🇪🇺 — ultra-rapide mais données hors souveraineté FR

         Tape a, b ou c.

Utilisateur : a

Claude : OVH ✓ — tes données resteront en France, à Gravelines.

         3️⃣ L'application encaisse-t-elle des paiements ? (acompte de
         réservation, par exemple)
         ...
```

À la fin, l'application de base est déployée et Claude propose d'ajouter
les fonctionnalités une par une.

---

## Structure du plugin

```
fastdeploy-plugin/
├── .claude-plugin/
│   ├── plugin.json        # Manifeste du plugin
│   └── mcp.json           # Connexion au serveur MCP
├── commands/
│   └── fastdeploy.md      # La commande /fastdeploy (le wizard)
└── skills/
    └── fastdeploy/
        └── SKILL.md       # Base de connaissances déploiement
```

## Comment ça marche

1. `/fastdeploy` déclenche le wizard conversationnel (défini dans `commands/fastdeploy.md`)
2. Claude pose les questions une par une, guidé par le skill `fastdeploy`
3. À la confirmation, Claude appelle les outils du serveur MCP FastDeploy :
   - `deployment_advisor` — valide l'architecture
   - `generate_files` — génère les configs
   - `ovh_provision` / `scaleway_provision` — crée l'infra réelle
4. L'app de base est en production, les features s'ajoutent par prompts

## Plans

| Plan | Wizard + conseils + fichiers | Provisioning infra réel |
|---|---|---|
| Enterprise | ✅ | ❌ (génère les fichiers à déployer soi-même) |
| Enterprise+ | ✅ | ✅ (déploiement automatique OVH/Scaleway) |
