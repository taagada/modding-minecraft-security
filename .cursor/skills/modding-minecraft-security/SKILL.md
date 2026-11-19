---
name: modding-minecraft-security
description: Conçois ou révise la sécurité d'un mod Minecraft Java ou de son serveur. Utilise ce skill pour l'autorité serveur, les paquets et menus, l'authentification, l'isolation proxy/backend et la détection anti-triche.
---

# Sécurité du modding Minecraft

Traite chaque donnée issue du client comme une déclaration. Le serveur décide des effets en jeu à partir de la session authentifiée, de ses permissions et de son propre état.

## Analyse fonctionnelle

1. Identifie la version Minecraft, le loader et les côtés logiques impliqués. Lis les enregistrements de paquets, leurs codecs et leurs handlers avant de tirer des conclusions à partir du GUI ou du code client.
2. Suis chaque action pertinente depuis son émetteur jusqu'à ses effets sur le monde, l'inventaire, la base ou la modération. Repère le thread d'exécution, la source de l'identité et les contrôles qui précèdent effectivement la mutation.
3. Vérifie les invariants applicables ci-dessous. En revue, cite les fichiers et lignes, explique la conséquence pour le joueur ou le service, puis donne une correction et un critère de vérification. Distingue les constats du code des hypothèses de déploiement.

## Paquets et menus

- Fais transmettre une intention minimale. Pour un achat, reçois le slot et le contexte de menu requis. Résous l'offre et le prix à partir du menu serveur, puis vérifie le solde et l'inventaire avant d'appliquer l'échange. Pour une attaque, calcule les dégâts à partir de l'arme, des effets et de la portée connus du serveur.
- Revalide l'identité du menu actif, le slot, la page ou la révision quand elle change la correspondance des slots. Filtre côté serveur les entrées que le joueur ne doit pas recevoir. Vérifie les longueurs, plages et règles métier des saisies.
- Borne le décodage avant les allocations, y compris les tailles cumulées et le nombre de transferts en cours. Rejette les messages mal formés, hors phase ou hors permission avant tout effet. Revalide l'état mutable au moment de l'action sur le thread prévu par le loader. Garde les opérations de débit et de transfert cohérentes en cas de doublon ou d'échec.

## Identité et infrastructure

- Identifie le joueur à partir de la connexion authentifiée par Minecraft ou d'un forwarding authentifié depuis un proxy de confiance. Vérifie les autorisations pour chaque commande sensible. Lie les preuves temporaires à la session et à leur usage. Donne-leur un TTL court et consomme-les atomiquement si elles sont à usage unique.
- Vérifie séparément l'identité du pair et l'intégrité du canal. Privilégie TLS avec validation du pair selon l'architecture. Si le protocole utilise déjà un chiffrement applicatif, vérifie le tag AEAD et l'unicité des nonces par clé pour AES-GCM.
- Limite l'accès public au proxy quand l'architecture en prévoit un. Vérifie au niveau réseau que les backends acceptent uniquement les sources attendues, et que RCON, le panel et la base sont sur des accès privés. Vérifie aussi les flux autorisés entre backends.
- Limite le débit par route et par source fiable, en tenant compte des joueurs qui partagent une IP. Ajoute un budget global ou de concurrence lorsque le coût du service l'exige. Distingue identité refusée et fournisseur indisponible. Définis explicitement les actions permises en mode dégradé et protège l'attribution des écritures à un compte.

## Détection anti-triche

- Superpose des contrôles à la connexion, en cours de session et lors de changements pertinents. Croise les rapports client avec les observations serveur et évalue quelles sources partagent le même point de défaillance. Un inventaire de classes en mémoire et une liste de fichiers peuvent diverger pour des raisons légitimes.
- Rejette les actions qui violent un invariant. Pour un indice anti-triche ambigu, conserve un diagnostic serveur, limite les détails visibles du client et prévois une revue humaine avant sanction selon la politique du projet. Distingue absence de rapport, panne et incohérence avérée. Limite la collecte et l'accès aux captures ou identifiants matériels.
- Traite l'obfuscation, la rotation des mappings et la télémétrie JVM comme des couches complémentaires. Garde les secrets et les décisions sensibles côté serveur.

Vérifie les changements avec des tests locaux ciblés sur les effets attendus, par exemple l'achat valide, le menu périmé sans débit, la preuve expirée sans attribution ou l'indisponibilité du service d'identité. Utilise des doubles pour les dépendances externes.