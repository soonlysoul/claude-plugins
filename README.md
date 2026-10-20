# Marketplace Claude Code `soon`

Le catalogue des plugins Claude Code de Soon. Il ne contient pas de code : chaque plugin vit dans son propre dépôt, Ce que ce catalogue référence.

| Plugin | Rôle | Dépôt |
| --- | --- | --- |
| `plugin-flutter` | Workflows pour projets Flutter et Dart : bootstrap, fonctionnalités en TDD, Git propre, reprise de session, GitHub (PR et CI), débogage, garde-fous | [soonlysoul/plugin-flutter](https://github.com/soonlysoul/plugin-flutter) |
| `plugin-nixos` | Configuration NixOS d'un poste : la trouver ou la reprendre de `/etc/nixos` dans un dépôt git, prouver une option avant de l'écrire, vérifier sans privilège, proposer sans appliquer, garde-fous | [soonlysoul/plugin-nixos](https://github.com/soonlysoul/plugin-nixos) |

## Installer sur un appareil

Prérequis : Claude Code 2.1.295 ou plus, et `git`. Chaque plugin a ses propres prérequis : `plugin-flutter` demande aussi `jq` et Flutter (et `gh` connecté pour GitHub, gitleaks recommandé), `plugin-nixos` demande un poste NixOS. Aucun compte GitHub n'est nécessaire, car les dépôts sont publics.

```bash
claude plugin marketplace add soonlysoul/claude-plugins
claude plugin install plugin-flutter@soon --scope user
claude plugin install plugin-nixos@soon --scope user
```

Sur un NixOS fraîchement installé, où ni Claude Code ni git ne sont encore déclarés, voir le [README de `plugin-nixos`](https://github.com/soonlysoul/plugin-nixos#installer).

En session : `/plugin marketplace add soonlysoul/claude-plugins`, puis `/plugin install plugin-flutter@soon`.

**Appareil sans clé SSH GitHub.** Pour un dépôt GitHub, Claude Code essaie d'abord SSH. Exportez `CLAUDE_CODE_PLUGIN_PREFER_HTTPS=1`, dans le profil du shell par exemple, pour passer directement par HTTPS. Sans cette variable, l'installation depuis `/plugin` et les mises à jour échouent sur un tel appareil.

Vérifier : `claude plugin list`, puis `claude plugin details plugin-flutter`.

## Mettre à jour

```bash
claude plugin marketplace update soon
claude plugin update plugin-flutter@soon
```

Pour que ce soit automatique : `/plugin` → Marketplaces → `soon` → *Enable auto-update*.

Un appareil ne reçoit une nouvelle copie d'un plugin que lorsque sa `version` change dans son `plugin.json`. Pousser des commits sans changer la version ne met donc personne à jour.

**Depuis `flutter-forge`.** Le plugin s'appelle `plugin-flutter` depuis la 0.1.2, et une mise à jour ne suit pas un renommage. Sur un appareil qui l'avait installé :

```bash
claude plugin marketplace update soon
claude plugin uninstall flutter-forge@soon --scope user
claude plugin install plugin-flutter@soon --scope user
```

## Désinstaller

```bash
claude plugin uninstall plugin-flutter@soon --scope user
claude plugin marketplace remove soon     # retire aussi les plugins installés depuis ce catalogue
```

## Ajouter un plugin

1. Publier le plugin dans son propre dépôt, avec un `.claude-plugin/plugin.json` dont `name` est en kebab-case.
2. Ajouter une entrée dans `.claude-plugin/marketplace.json`, avec **le même `name`** que celui du `plugin.json` :

   ```json
   {
     "name": "mon-plugin",
     "source": { "source": "github", "repo": "soonlysoul/mon-plugin" },
     "description": "Une ligne affichée dans /plugin."
   }
   ```

   Ne pas répéter `version` dans l'entrée : celle du `plugin.json` fait foi.
3. Valider, puis pousser :

   ```bash
   claude plugin validate .
   ```

4. Sur chaque appareil : `claude plugin marketplace update soon`, puis `claude plugin install mon-plugin@soon`.

Un plugin hébergé dans un dépôt privé peut figurer ici, mais chaque appareil devra alors disposer d'identifiants GitHub, par exemple avec `gh auth login` puis `gh auth setup-git`.

## Licence

MIT, voir [LICENSE](LICENSE). Chaque plugin a sa propre licence dans son dépôt.
