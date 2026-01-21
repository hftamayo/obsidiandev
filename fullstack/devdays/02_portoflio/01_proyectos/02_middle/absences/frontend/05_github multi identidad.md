
## Flow:

- profile A: es el maintainer del codebase
- profile B: es el DevOps profile, no hará modificaciones al codigo
- flow: profileA actualiza -> profile B descarga cambios -> replica a su repoB -> ejecuta operaciones DevOps


## comandos:

```
git clone git@github.com:hftamayo/absencesbo-devops.git
git remote rename origin upstream
git remote add origin https://github.com/htues/absencesbo-devops.git
git remote set-url origin git@github.com:htues/absencesbo-devops.git
git remote -v
# 1. Fetch and merge latest from repo A
git pull upstream main

# 2. Push the updates to repo B
git push origin main

upstream = repo A (source of truth, read-only for Profile B)
origin = repo B (where Profile B pushes for DevOps)

```

## Previamente el directorio ssh debe tener las keys descargadas y un archivo config que se ve asi:

```

# GitHub account 1 (hftamayo)
Host github.com-hftamayo
  HostName github.com
  User git
  IdentityFile ~/.ssh/hftamayo_dell_ed25519

# GitHub account 2 (htues)
Host github.com-htues
  HostName github.com
  User git
  IdentityFile ~/.ssh/htues_dell_ed25519

```

