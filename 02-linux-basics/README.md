# 02 — Linux : bases

## Objectif

Se familiariser avec l'arborescence Linux, la lecture des permissions, et la gestion basique des utilisateurs — prérequis pour tout le reste du cours (serveurs, Docker, Ansible...).

## Arborescence

Contrairement à Windows/macOS, Linux organise tout (fichiers, dossiers, périphériques) dans une seule arborescence partant de la racine `/`.

Dossiers clés :
- `/home/<user>` — dossier personnel
- `/etc` — fichiers de configuration système
- `/var` — données variables (logs, fichiers temporaires)

## Lecture de `ls -la`

```
drwxr-x--- 4 vagrant vagrant 4096 Jul 15 19:00 .
-rw------- 1 vagrant vagrant   19 Jul 15 19:00 .bash_history
-rw-r--r-- 1 vagrant vagrant  220 Jul 11 11:31 .bash_logout
```

- 1er caractère : type (`d` = dossier, `-` = fichier, `l` = lien symbolique)
- 9 caractères suivants, groupés par 3 : permissions **propriétaire / groupe / autres**
  - `r` (read), `w` (write), `x` (execute), `-` (absent)

Exemple concret observé : `/etc/shadow` (mots de passe chiffrés du système) a la permission `rw-r-----` — lecture/écriture pour `root` uniquement, lecture pour le groupe `shadow`, rien pour tout le reste. Illustration directe du principe de moindre privilège.

## Permissions — `chmod`

Deux notations possibles :

```bash
chmod u+x script.sh      # notation symbolique
chmod 755 script.sh      # notation numérique (r=4, w=2, x=1)
```

Valeurs courantes pratiquées :
| Valeur | Résultat | Usage typique |
|---|---|---|
| `600` | `rw-------` | fichiers sensibles (clés, historique) |
| `644` | `rw-r--r--` | fichiers texte / config partageables |
| `755` | `rwxr-xr-x` | scripts exécutables |

## Utilisateurs et privilèges

```bash
passwd                          # change le mot de passe de l'utilisateur courant
sudo passwd <user>              # change le mot de passe d'un autre utilisateur (droits root requis)
sudo adduser <user>             # crée un nouvel utilisateur
sudo usermod -aG sudo <user>    # ajoute un utilisateur au groupe sudo (droits admin)
```

`sudo` ("superuser do") permet d'emprunter temporairement les privilèges de `root` pour une seule commande, sans changer d'utilisateur connecté :

```bash
whoami       # vagrant
sudo whoami  # root
```

## Ce que j'ai appris

- Les permissions par défaut d'un fichier reflètent sa sensibilité (ex: `.ssh`, `/etc/shadow` très restreints vs fichiers de config lisibles par tous).
- `sudo` ne change pas d'utilisateur connecté, il élève temporairement les privilèges pour une commande.
- Éditeurs en ligne de commande pratiqués : `nano` (simple) et `vim` (modes normal/insertion, `:wq` pour sauvegarder-quitter).
