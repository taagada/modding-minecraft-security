# Sécurité du modding Minecraft

Un [Agent Skill](https://agentskills.io/) en français pour concevoir ou relire la sécurité d’un mod Minecraft Java ou de son serveur. Il couvre l’autorité serveur, les paquets et menus, l’authentification, l’isolation proxy/backend et la détection anti-triche.

## Installation

Chaque assistant dispose de son arborescence dans ce dépôt. Pour installer le skill dans un autre projet Minecraft, copie l’arborescence indiquée à la racine de ce projet, puis ouvre une nouvelle session de l’assistant.

| Assistant | Arborescence dans le dépôt | Documentation |
| --- | --- | --- |
| Codex | [`.agents/skills/`](.agents/skills/) | [OpenAI](https://developers.openai.com/api/reference/cli) |
| Claude Code | [`.claude/skills/`](.claude/skills/) | [Anthropic](https://code.claude.com/docs/en/skills) |
| GitHub Copilot CLI | [`.github/skills/`](.github/skills/) | [GitHub](https://docs.github.com/en/copilot/how-tos/copilot-cli/customize-copilot/add-skills) |
| Gemini CLI | [`.gemini/skills/`](.gemini/skills/) | [Google](https://geminicli.com/docs/cli/skills/) |
| Cursor | [`.cursor/skills/`](.cursor/skills/) | [Cursor](https://cursor.com/docs/skills) |

Dans chaque arborescence, le fichier se trouve sous `modding-minecraft-security/SKILL.md`. Codex, GitHub Copilot CLI, Gemini CLI et Cursor peuvent aussi partager une seule copie dans `.agents/skills/`.

Dans un chat sans prise en charge des Agent Skills, joins le [fichier `SKILL.md`](.agents/skills/modding-minecraft-security/SKILL.md) ou colle son contenu, puis fournis les extraits du projet à analyser.

## Exemple

> Utilise le skill `modding-minecraft-security` pour relire le menu d’achat de mon mod Fabric. Vérifie que le serveur détermine le prix et qu’un menu périmé ne peut déclencher aucun débit. Cite les fichiers et propose les corrections nécessaires.

## Maintenance

Modifie le [fichier source du skill](.agents/skills/modding-minecraft-security/SKILL.md), puis synchronise les copies depuis la racine du dépôt avec PowerShell.

```powershell
./scripts/Sync-Skills.ps1
./scripts/Sync-Skills.ps1 -Check
```

La première commande régénère les copies et la seconde vérifie qu’elles sont identiques à la source. Les assistants lisent directement les fichiers Markdown.
