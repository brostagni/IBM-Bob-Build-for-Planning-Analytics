# Connecter IBM Bob au MCP Planning Analytics (TechZone)

> Guide de configuration du MCP Planning Analytics dans IBM Bob v2.x  
> Testé sur : IBM Bob v2.0.0 — PA TechZone (on-premise)

---

## Prérequis

- IBM Bob v2.x installé
- Accès à un environnement Planning Analytics (TechZone ou SaaS)
- Les informations de connexion suivantes :
  - **PA Host URL** (ex. `http://eu-de.services.cloud.techzone.ibm.com:31497`)
  - **Tenant ID** (ex. `EZXHLSXGX9VK`)
  - **Username / Password** (ou API Key pour SaaS)

---

## Étape 1 — Encoder les credentials en Base64

Le header Authorization attend un encodage Base64 du format `username:password`.

Dans un terminal :

```bash
echo -n "pm:IBMDem0s" | base64
# Résultat : cG06SUJNRGVtMHM=
```

> **Pour SaaS** : le format est `apikey:TA_CLE_API`  
> **Pour TechZone / on-premise** : le format est `username:password`

---

## Étape 2 — Ouvrir le panneau MCP dans Bob

1. Cliquer sur l'icône **⚙️ Settings** (coin supérieur droit)
2. Dans la colonne gauche, cliquer sur **MCP**
3. Cliquer sur le bouton **Install** en face de **Planning Analytics**

---

## Étape 3 — Remplir le formulaire d'installation

Le formulaire demande 3 champs :

| Champ | Valeur (exemple TechZone) |
|---|---|
| **Planning Analytics Host URL** | `http://eu-de.services.cloud.techzone.ibm.com:31497/api/EZXHLSXGX9VK/` |
| **Tenant ID** | `EZXHLSXGX9VK` |
| **Authorization value** | `Basic cG06SUJNRGVtMHM=` |

> ⚠️ Le champ Authorization doit inclure le préfixe `Basic ` (avec l'espace)

Cliquer sur **Install**.

---

## Étape 4 — Corriger le fichier de config généré

Bob v2 génère parfois une URL malformée (duplication du path, `https://` devant `http://`).  
Il faut éditer manuellement le fichier `.bob/mcp.json` dans le dossier du workspace.

### Fichier corrigé complet

```json
{
  "mcpServers": {
    "pa-tz-cube": {
      "type": "streamable-http",
      "url": "http://eu-de.services.cloud.techzone.ibm.com:31497/api/EZXHLSXGX9VK/v0/agentic-ai/cube/mcp",
      "headers": {
        "Authorization": "Basic cG06SUJNRGVtMHM="
      },
      "timeout": 600000,
      "alwaysAllow": [
        "get_cubes_that_may_answer_query",
        "get_available_tm1_servers",
        "get_data_from_data_explorer",
        "lookup_potential_members",
        "get_cube_sample_members"
      ],
      "disabled": false
    },
    "pa-tz-server": {
      "type": "streamable-http",
      "url": "http://eu-de.services.cloud.techzone.ibm.com:31497/api/EZXHLSXGX9VK/v0/agentic-ai/analysis/mcp",
      "headers": {
        "Authorization": "Basic cG06SUJNRGVtMHM="
      },
      "timeout": 600000,
      "alwaysAllow": [
        "get_available_tm1_servers"
      ],
      "disabled": false
    }
  }
}
```

### Les deux endpoints MCP disponibles

| Endpoint | URL | Usage |
|---|---|---|
| **Cube tools** | `/v0/agentic-ai/cube/mcp` | Exploration cubes, dimensions, données, MDX |
| **Server tools** | `/v0/agentic-ai/analysis/mcp` | Serveurs TM1, processus, administration |

### Pourquoi `timeout: 600000` ?

Le timeout est en **millisecondes**. Les serveurs TechZone peuvent mettre 15-30s à répondre.  
La valeur par défaut de Bob (60 000 ms = 60s) est souvent insuffisante.  
`600000` = 10 minutes — largement suffisant.

---

## Étape 5 — Redémarrer Bob

1. **Fermer** Bob complètement (Cmd+Q sur Mac)
2. **Relancer** Bob
3. Aller dans **Settings → MCP**

Les deux serveurs doivent apparaître avec le statut 🟢 **Connected** :

| Name | Status | Scope |
|---|---|---|
| TM1 Cube Service | 🟢 Connected | Workspace |
| AnalysisServer | 🟢 Connected | Workspace |

---

## Étape 6 — Tester la connexion

Dans une nouvelle conversation Bob :

```
What TM1 servers are available?
```

Bob appelle automatiquement l'outil `get_available_tm1_servers` et retourne la liste des serveurs TM1.

---

## Adaptation pour d'autres environnements

### PA SaaS (IBM Cloud)

```json
{
  "mcpServers": {
    "pa-saas-cube": {
      "type": "streamable-http",
      "url": "https://us-east-1.planninganalytics.saas.ibm.com/api/{TENANT_ID}/v0/agentic-ai/cube/mcp",
      "headers": {
        "Authorization": "Basic {BASE64_DE_apikey:TA_CLE_API}"
      },
      "timeout": 600000,
      "alwaysAllow": [
        "get_cubes_that_may_answer_query",
        "get_available_tm1_servers",
        "get_data_from_data_explorer",
        "lookup_potential_members",
        "get_cube_sample_members"
      ],
      "disabled": false
    }
  }
}
```

> Le Tenant ID se trouve dans l'URL de PA Workspace :  
> `https://us-east-1.planninganalytics.saas.ibm.com/?accountId=XXXX&tenantId=TENANT_ID`

---

## Troubleshooting

| Problème | Cause | Solution |
|---|---|---|
| `Disconnected` après Install | URL malformée par Bob v2 | Éditer `.bob/mcp.json` manuellement |
| `Request timed out` | Timeout trop court | Passer `timeout` à `600000` |
| `401 Unauthorized` | Base64 incorrect | Re-encoder avec `echo -n "user:pass" \| base64` |
| `403 Forbidden` | Droits insuffisants | Vérifier l'entitlement **Planning Analytics Agent** |
| `Failed to connect` | URL inaccessible | Vérifier VPN / réseau / port ouvert |

### Vérifier que le serveur répond (curl)

```bash
curl -v \
  -H "Authorization: Basic cG06SUJNRGVtMHM=" \
  "http://eu-de.services.cloud.techzone.ibm.com:31497/api/EZXHLSXGX9VK/v0/agentic-ai/cube/mcp"
```

Réponse attendue : `HTTP/1.1 200 OK` avec `content-type: text/event-stream`

---

## Emplacement des fichiers de config

| Scope | Chemin | Effet |
|---|---|---|
| **Workspace** | `.bob/mcp.json` | Disponible uniquement dans ce projet |
| **Global** | `~/.bob/settings/mcp.json` | Disponible dans toutes les conversations |

---

*Dernière mise à jour : Juillet 2026 — Bob v2.0.0 / PA TechZone*
