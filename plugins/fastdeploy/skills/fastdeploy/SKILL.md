---
name: fastdeploy
description: Connaissances de déploiement souverain pour le wizard conversationnel FastDeploy. Matrice de décision plateforme, patterns d'authentification, baseline cybersécurité CAC 40. Utilisé par la commande /fastdeploy pour répondre avec précision aux choix de l'utilisateur.
---

# FastDeploy — Base de connaissances

Ce skill alimente le wizard `/fastdeploy` avec l'expertise nécessaire pour
conseiller l'utilisateur à chaque étape.

## Matrice de décision plateforme

| Contexte | Recommandation | Pourquoi |
|---|---|---|
| Données de santé | OVH HDS certifié | Obligation légale hébergeur HDS |
| Secteur public / OIV | OVH SecNumCloud | Qualification ANSSI requise |
| Données personnelles FR sensibles | OVH ou Scaleway | Souveraineté, données en France |
| Startup / app grand public | Vercel EU + OVH BDD | Rapidité de déploiement |
| Groupe avec écosystème Microsoft | Azure EU | Intégration native |

**Alerte à toujours donner** : Vercel n'est PAS conforme HDS ni SecNumCloud.
Si l'utilisateur mentionne santé ou secteur public, orienter fermement vers OVH.

## Patterns d'authentification par niveau

| Niveau | Solution | Pour qui |
|---|---|---|
| Grand public | Email + mot de passe + rate limiting | Sites vitrine, e-commerce simple |
| Entreprise | Keycloak OIDC + MFA TOTP | SaaS B2B, ETI |
| Grand groupe | SSO SAML/OIDC + MFA + audit trail | CAC 40 |
| Maximum | FIDO2 / Passkeys (clés matérielles) | OIV, données classifiées |

## Baseline cybersécurité par niveau

### Standard (RGPD)
TLS 1.2+, HTTPS partout, headers de sécurité, validation des entrées,
hash bcrypt, RGPD (registre traitements, consentement).

### Entreprise (CAC 40)
Tout le Standard +
- WAF avec règles OWASP
- Scan dépendances automatique (Snyk/Dependabot)
- SBOM CycloneDX au build
- Séparation dev/staging/prod
- MFA admins obligatoire
- Logs auth rétention 1 an
- Pentest annuel PASSI
- DPA sous-traitants

### Souverain (SecNumCloud / OIV)
Tout l'Entreprise +
- Hébergement SecNumCloud (OVH)
- Chiffrement de bout en bout
- SIEM centralisé, logs 3 ans
- Isolation réseau totale
- Déclaration ANSSI (LPM)
- Qualification logiciels critiques

## Templates d'application de base

Selon le type choisi, l'app de base déployée contient :

| Type | Stack de base | Inclut |
|---|---|---|
| Web | Next.js + Tailwind | Page d'accueil, structure, health check |
| API | Express/FastAPI | Endpoints de base, doc OpenAPI, health check |
| Full-stack | Next.js + PostgreSQL | Front + API + BDD + auth de base |

L'app de base est volontairement minimale. Les fonctionnalités métier
(réservation, menu, paiement...) s'ajoutent ensuite par prompts successifs.

## Référentiels de conformité

RGPD (tous), ANSSI (souverain), NIS2 (entreprises essentielles),
ISO 27001 (recommandé), HDS (santé), SecNumCloud (public/OIV),
DORA (finance), PCI-DSS (si paiement par carte).

## Ton et posture

Le wizard doit être :
- **Chaleureux mais expert** — comme un architecte cloud senior bienveillant
- **Concis** — une question à la fois, pas de pavés
- **Pédagogue** — expliquer brièvement le "pourquoi" de chaque recommandation
- **Honnête** — si Vercel ne convient pas pour de la santé, le dire clairement
