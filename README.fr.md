<div align="center">

# Daimon

**Des yeux et des mains pour n'importe quel client IA — sur votre ordinateur, à vos conditions.**

Une application locale qui permet aux outils IA que vous utilisez déjà de voir votre écran et d'y agir, par le Model Context Protocol. C'est vous qui fixez jusqu'où ils peuvent aller.

[![Dernière version](https://img.shields.io/github/v/release/ArboRithm/Daimon?label=version&color=B66CFF)](https://github.com/ArboRithm/Daimon/releases/latest) [![Plateforme](https://img.shields.io/badge/macOS%20%C2%B7%20Windows-B66CFF?label=plateforme)](#télécharger)

[**Télécharger**](https://github.com/ArboRithm/Daimon/releases/latest) · [English](README.md)

</div>

## Ce qu'est Daimon

Les assistants IA raisonnent bien et ne voient rien de votre bureau. Daimon est l'organe qui leur manque : une petite application résidente qui expose la **perception** (l'écran, la structure des fenêtres) et l'**action** (souris, clavier, fenêtres) à tout client IA qui parle le [Model Context Protocol](https://modelcontextprotocol.io).

Daimon est un outil, pas un assistant. Il répond quand un client IA l'interroge, jamais l'inverse : **il n'appelle lui-même aucun modèle d'IA**, n'a pas de compte et n'envoie rien à ArboRithm. Ce que votre client IA voit à travers Daimon, il le traite selon ses propres conditions.

## Les organes

| Organe | Ce qu'il apporte à l'IA | Outils |
|---|---|---|
| **Vue** | Des captures d'un écran, renvoyées directement ; la localisation d'un texte à l'écran, calculée sur l'appareil (OCR : Apple Vision sous macOS, OCR de Windows sous Windows) ; la géométrie des écrans et leur calibration | `vue_*` |
| **Touché** | L'arbre d'accessibilité d'une fenêtre (accessibilité macOS, UI Automation sous Windows), ou l'élément situé sous un point | `touche_*` |
| **Mains** | Cliquer, saisir, appuyer, glisser, naviguer, combinaisons de touches, contrôle des fenêtres et des applications | `main_*` |
| **Overlay** — montrer | Des repères à l'écran pour suivre ce que fait l'IA : surbrillance, projecteur, halo de curseur, bandeau. Désactivé par défaut ; il s'active dans le panneau (**Présence à l'écran**) | `overlay_*` |
| **Face** — l'application | L'icône de la barre de menus (macOS) ou de la zone de notification (Windows), son panneau et le parcours d'installation : autorisations, clients IA, plafond des Mains, mises à jour | — |

Un diagnostic en lecture seule, `daimon_doctor`, dit en un appel quelles autorisations sont accordées (macOS), ce que casse chacune de celles qui manquent, et quel plafond est actif.

## Vous gardez la main

### Le plafond des Mains

Chaque action a un niveau. Le plafond que vous fixez est le niveau le plus élevé qu'une IA peut utiliser ; tout ce qui le dépasse est refusé. **Une IA peut lire le plafond, jamais le relever.**

| Niveau | Nom | Ce qu'il permet |
|---|---|---|
| **L0** | Lecture | Rien d'autre que la perception. **C'est le réglage par défaut.** |
| **L1** | Sans risque | Les actions réversibles — focus, fenêtre, défilement, navigation |
| **L2** | Saisie | Clics, saisie de texte, glisser, combinaisons de touches |
| **L3** | Validation | Les boutons qui engagent — envoyer, confirmer |
| **L4** | Autonomie | Tout, sans confirmation action par action |

Vous déplacez le plafond entre L0 et L3 avec le curseur du panneau de Daimon. Jusqu'à L3, une action que Daimon juge irréversible — ou que l'IA déclare telle — s'arrête sur une boîte de confirmation, qui refuse d'elle-même au bout de 30 secondes.

**L4 demande votre consentement explicite.** Il s'engage depuis le panneau (une boîte de confirmation) ou, sous macOS, depuis un terminal en tapant une phrase entière (`/Applications/Daimon.app/Contents/MacOS/Daimon consent engage`) ; aucun fichier de configuration ne permet de l'atteindre, et aucun outil ne permet à une IA de l'engager. Il reste actif jusqu'à ce que vous le désengagiez.

### Les secrets

Les champs de mot de passe sont masqués dans ce que l'IA lit de l'arbre d'accessibilité. Dans les captures d'écran ils sont noircis aussi, lorsque l'arbre d'accessibilité est lisible (sous macOS, une fois l'autorisation Accessibilité accordée) — une passe faite au mieux, et le résultat le signale quand elle n'a pas pu avoir lieu. Vous pouvez ajouter vos propres exclusions (`exclusions.yaml` dans le dossier de configuration) : des applications dont la présence au premier plan fait refuser la capture, des zones de l'écran toujours noircies, des titres de fenêtres retirés de l'arbre et refusés comme cibles.

### Une trace de ce qui a été fait

Chaque action des Mains est inscrite, sur votre ordinateur, dans un journal dont toute altération se détecte (chaque entrée est chaînée à la précédente). Il consigne ce qui a été fait et sur quoi — jamais le texte saisi. **Journal d'activité**, dans le panneau, ouvre son dossier.

## Télécharger

| Plateforme | Configuration | Fichier |
|---|---|---|
| **macOS** | Apple Silicon, macOS 12 ou plus récent | `Daimon_<version>_aarch64.dmg` |
| **Windows** | Windows 10 version 2004 ou plus récent, x64 | `Daimon_<version>_x64-setup.exe` |
| **Windows** (préversion) | Windows 10 version 2004 ou plus récent, ARM64 | `Daimon_<version>_arm64-setup.exe` |

Il est dans la [dernière release](https://github.com/ArboRithm/Daimon/releases/latest). La version macOS est signée et notarisée par Apple.

Les installateurs Windows ne sont pas signés Authenticode : SmartScreen peut donc afficher un avertissement — cliquez sur **Informations complémentaires**, puis sur **Exécuter quand même**. Le support de Windows ARM64 est une préversion : il n'a pas encore été testé sur une vraie machine ARM.

## Prise en main

1. **Installez** — sous macOS, ouvrez le DMG et glissez Daimon dans **Applications** ; sous Windows, lancez l'installateur. Faites-le avant la suite : vos clients IA sont enregistrés avec l'emplacement de l'application.
2. **Lancez Daimon.** Il vit dans la barre de menus sous macOS (pas d'icône dans le Dock) et dans la zone de notification sous Windows, près de l'horloge — éventuellement dans le menu de débordement. Cliquez sur son icône pour ouvrir le panneau.
3. Choisissez **Relancer l'installation…** dans le panneau. Le parcours vous guide dans les autorisations (sous macOS) et vos clients IA.
4. **Redémarrez votre client IA**, puis demandez-lui de regarder votre écran.

### Autorisations

Sous macOS, Daimon a besoin de deux autorisations, toutes deux dans **Réglages Système › Confidentialité et sécurité** :

- **Enregistrement de l'écran** — pour voir l'écran (Vue).
- **Accessibilité** — pour lire la structure des fenêtres et pour agir (Touché et les Mains).

Sous Windows, aucune autorisation système n'est à accorder.

Un point à connaître : macOS rattache ces autorisations à l'application qui *lance* Daimon — c'est-à-dire votre client IA (son application de bureau, votre terminal, votre éditeur). Si un outil échoue, demandez à votre IA d'appeler `daimon_doctor` : il nomme l'application à laquelle accorder l'autorisation.

### Clients IA

Daimon s'enregistre comme serveur MCP nommé `daimon` dans les clients qu'il détecte sur votre ordinateur — depuis le parcours d'installation, depuis le panneau (un interrupteur par client), ou depuis un terminal :

```sh
/Applications/Daimon.app/Contents/MacOS/Daimon status      # ce qui est détecté, ce qui est enregistré
/Applications/Daimon.app/Contents/MacOS/Daimon install     # s'enregistrer dans chaque client détecté
/Applications/Daimon.app/Contents/MacOS/Daimon uninstall   # se retirer
```

Sous Windows, le programme est `%LOCALAPPDATA%\Daimon\Daimon.exe` ; dans PowerShell, redirigez-le vers `Out-Host` pour que l'invite attende sa réponse :

```powershell
& "$env:LOCALAPPDATA\Daimon\Daimon.exe" status | Out-Host
```

Il connaît la configuration de : Claude Code, Claude Desktop, Codex, Gemini CLI, Antigravity, Cursor (éditeur et CLI), Windsurf, VS Code avec Copilot, GitHub Copilot CLI, Zed, JetBrains Junie, Cline, Roo Code, Continue, Amazon Q Developer CLI, opencode, Crush, Mistral Vibe, Qwen Code, Droid, Grok CLI, Goose, LM Studio, 5ire, Witsy, AnythingLLM Desktop, OpenDesign et Trae. Tous n'ont pas été éprouvés de bout en bout — si l'un d'eux se comporte mal, [ouvrez une issue](https://github.com/ArboRithm/Daimon/issues).

Tout autre client MCP peut aussi utiliser Daimon : déclarez un serveur stdio dont la commande est `/Applications/Daimon.app/Contents/MacOS/Daimon` (macOS) ou `%LOCALAPPDATA%\Daimon\Daimon.exe` (Windows), avec l'unique argument `serve`.

## Mises à jour

Daimon vérifie s'il existe une nouvelle version au lancement, à l'ouverture de son panneau, et une fois par jour, et l'affiche dans le panneau. Rien ne s'installe tant que vous n'avez pas cliqué sur **Mettre à jour vers la v…**. La signature de chaque mise à jour est vérifiée avant qu'elle ne remplace l'application.

Pour désactiver la vérification automatique, placez ceci dans un fichier nommé `update.yaml` dans le dossier de configuration (panneau › **Ouvrir le dossier de configuration**) :

```yaml
update:
  enabled: false
```

## Confidentialité

Daimon tourne sur votre ordinateur. Il n'a ni télémétrie ni compte, et ne collecte rien. Sa seule connexion à Internet est la vérification des mises à jour, servie par GitHub Releases. Si l'outil GitHub CLI (`gh`) est installé et connecté, cette vérification lui demande aussi, avec votre propre session, si votre compte a accès au canal de mise à jour réservé d'ArboRithm — rien n'est transmis à ArboRithm.

Ce que votre client IA fait de ce qu'il perçoit à travers Daimon — une capture d'écran, le texte d'une fenêtre — se joue entre vous et le fournisseur de ce client. Réglez le plafond, et vos exclusions, en conséquence.

## Aide

- **Bugs et suggestions** — [ouvrez une issue](https://github.com/ArboRithm/Daimon/issues)
- **Contact** — app@arborithm.fr

---

<sub>Daimon est un logiciel gratuit édité par ArboRithm, distribué selon les termes de son [contrat de licence](LICENSE.md). Les composants tiers sont listés dans [THIRD-PARTY-NOTICES.txt](THIRD-PARTY-NOTICES.txt). Les versions antérieures de Daimon, écrites en Python, ont été publiées sous licence GNU AGPL-3.0, qui continue de s'appliquer à elles.</sub>
