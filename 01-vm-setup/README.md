# 01 — VM Setup : Vagrant + VirtualBox

## Objectif

Mettre en place un environnement Linux local, reproductible, pour pratiquer les exercices du cours sans dépendre du cloud (donc sans coût, et avec un droit à l'erreur illimité).

## Contexte / choix

Deux VM créées manuellement dans VirtualBox au départ (clics dans l'interface graphique : RAM, CPU, réseau configurés à la main). Problème : pas reproductible, pas versionnable, et sujet au "configuration drift" (la VM s'écarte progressivement de son état propre au fil des manipulations).

Bascule vers **Vagrant** : la VM est décrite dans un fichier texte (`Vagrantfile`), versionnable dans Git. Une commande recrée l'environnement à l'identique.

## Stack

- VirtualBox 7.2.8 (hyperviseur)
- Vagrant 2.4.9 (orchestration de la VM)
- Box : `ubuntu/jammy64`, mise à niveau vers Ubuntu 24.04 LTS en cours de route

## Commandes clés

```bash
# Initialiser le projet (génère le Vagrantfile)
vagrant init ubuntu/jammy64

# Créer et démarrer la VM à partir du Vagrantfile
vagrant up

# Se connecter en SSH dans la VM
vagrant ssh

# Éteindre la VM (conserve l'état)
vagrant halt

# Détruire complètement la VM (repart de zéro au prochain "vagrant up")
vagrant destroy
```

## VS Code Remote-SSH

Pour éditer les fichiers de la VM avec un vrai confort graphique (pas seulement `nano`/`vim`), connexion de VS Code directement à la VM via l'extension **Remote-SSH**.

Config SSH générée automatiquement par Vagrant :
```bash
vagrant ssh-config
```

Résultat ajouté à `~/.ssh/config` sur la machine hôte, puis connexion via la palette de commandes VS Code : `Remote-SSH: Connect to Host...`.

**Point d'attention** : deux mécanismes de communication VM ↔ hôte à ne pas confondre :
- **Dossier partagé** (`/vagrant` dans la VM ↔ dossier du projet sur l'hôte) : synchronisation automatique, sans SSH.
- **Remote-SSH** : accès à tout le système de fichiers de la VM (`/home/vagrant`, etc.), mais uniquement quand la connexion SSH est active.

## Ce que j'ai appris

- Le principe de virtualisation : hyperviseur (VirtualBox) simulant un ordinateur complet (VM invitée) au-dessus d'un OS hôte (macOS).
- Pourquoi automatiser la création de VM plutôt que cliquer : reproductibilité, droit à l'erreur gratuit, versionning.
- Différence entre `VBoxManage` (pilotage direct de VirtualBox) et `vagrant` (orchestration au-dessus, à partir d'un fichier de config).
