# Cloner un répo privé via SSH sur Ubuntu
*Auteur : Anicet Nougaret
Dernière maj : 07/03/2021*

## Prérequis
- Ubuntu
- git
- Un compte utilisateur sudo
- Un compte github et un repo privé
- Un email

## Modificateurs
- Remplacer `<EMAIL>` par votre email
- Remplacer `<user>` par votre compte utilisateur sur Ubuntu
- Remplacer `<clone_path>` par le chemin absolu vers le dossier où vous voulez cloner
- Remplacer `<repo_ssh_url>` par l'url ssh de votre repo (commence par `git@github`)

# Recette
- Passer sur le bon compte : `su <user>`
- Générer la clé ssh : `ssh-keygen -t ed25519 -C "<EMAIL>"`
- Lancer ssh-agent :
    ```bash
    $ eval "$(ssh-agent -s)"
    > Agent pid 59566
    ```
- Ajouter la clé au ssh-agent : `ssh-add ~/.ssh/id_ed25519`
- Afficher et copier la clé publique : `cat ~/.ssh/id_ed25519.pub`
- Mettre la clé dans votre répo github dans `Settings > Deploy Keys`
- Cloner le repo là où vous le voulez, mais surtout **sans sudo**, si vous clonez avec sudo alors ssh utilisera vos clés root, pas les clés de `<user>` qu'on vient de créer !
    ```bash
    $ git clone <repo_ssh_url> <clone_path>
    > Cloning into '<clone_path>'...
      remote: Enumerating objects: 106, done.
      remote: Counting objects: 100% (106/106), done.
      remote: Compressing objects: 100% (80/80), done.
      remote: Total 106 (delta 20), reused 102 (delta 16), pack-reused 0
      Receiving objects: 100% (106/106), 122.76 KiB | 735.00 KiB/s, done.
      Resolving deltas: 100% (20/20), done.
    ```
  - Si vous devez sudo là où vous voulez cloner, changez les permissions du dossier plutôt : `sudo chown -R <user> <clone_path>`